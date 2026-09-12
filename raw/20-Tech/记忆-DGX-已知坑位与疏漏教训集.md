> 来源：WorkBuddy 记忆（已脱敏，隐去地址/账号/密码/Key/域名/人名）

---
title: DGX 已知坑位与疏漏教训集
aliases:
  - DGX 坑位清单
  - 工程教训集
  - 假成功 死循环 权限 幻觉
tags:
  - 工程教训
  - 踩坑
  - 排障
  - 假成功
  - 权限
  - 幻觉
category: raw
created: 2026-09-12
updated: 2026-09-12
sources:
  - WorkBuddy 记忆 ⚠️ 坑/教训/假成功/死循环/权限不足/幻觉 类条目
---

# DGX 已知坑位与疏漏教训集（脱敏）

> 把所有「⚠️」「坑」「教训」「假成功」「死循环」「权限不足」「幻觉」类内容集中提炼为工程教训清单，保留技术根因。

## 一、Edit/写盘"假成功"（最高频，反复复发）
- **Edit 工具会假成功不落盘**：报告成功但文件内容没真正写入（尤其 import 行、新增函数定义）。运行时才报 `xxx is not defined`。
- **教训**：**每个改动点都必须 grep 复核**，且**不能只 grep 调用处**——曾只复核了调用处没复核 import，import 行假成功未落盘导致运行时错误。
- **防空**：改完 `grep 复核 + node --check` 再部署。

## 二、向量库/RAG 数据坑
- ⚠️ **JS 处理 Qdrant u64 id 丢精度**：失败列表里记录的 id 字面量≠真身，按 id 反查必 miss。补漏要靠集合级重跑（超长文本截断重嵌、同 id 覆盖幂等），不能指望按 id 定位。
- **embed 超 bge-m3 上下文**是批量失败的根因（>5000 字符须截 3000 重嵌）。
- **Qdrant keyword 过滤假 0 命中**：用 `match.text` 分词命中为 0；必须 `match.value`。
- **两源集合 id 撞车覆盖**：合并两个代码来源库时少 15 点=撞车覆盖，属预期但要确认。

## 三、模型/harness 坑（幻觉与截断）
- **num_ctx 过低 → Ollama 静默截断工具 schema → 模型凭空造工具名**（曾造出 task_list / odoo_query 等不存在的工具名，调用必失败）。
- **num_ctx 严禁 128K**（并发 OOM）；硬限 16384~32768。
- **Qwen3-Coder 消息以 role:'tool' 结尾会 500**：工具结果必须以 user 消息回传。
- **机制工具名幻觉**：agent 系统提示词里强调"工具名必须严格照抄系统提供清单、一个字不能改"，但根本防线是 num_ctx 不能过低导致 schema 被截断。
- **LLM 大金额偶发数零**：4.5e9 答成 4.5 亿；金额一律以结构化返回体为准。
- **qwen3-vl 是思考模型**：num_predict 被 message.thinking 吃掉 → 正文空；须放大 num_predict。
- **身份反转/自称**：曾问"你是谁"答成用户名 / 自称通义千问（qwen3-coder 只认首条 system）；教名字遇"我叫 X"句式被记成用户自称——教学要用"以后你的名字就是 X"。

## 四、部署/环境坑
- **本机 `C:/Windows/System32/OpenSSH/ssh.exe` 任何命令返回 255 零输出**：进程级故障，debug 白耗大量时间；直接换 Git 版 OpenSSH `C:/Program Files/Git/usr/bin/ssh.exe`。同故障期伴生 shim 报 `dirname/ls: command not found`、PowerShell 输出丢失、python 崩溃（node 正常）。
- **ssh 嵌套引号复合命令必炸**：复杂操作写本地脚本 scp 上去执行。
- **命令行明文 key 会触发敏感审批卡死**：脚本内从容器 env 实时读 KEY。
- **compose 重建后 docker cp 归零**：容器代码没固化到镜像，重建即丢；odoo-web 重建还需重装 openpyxl/pptx/opencc/libreoffice。
- **宿主源码快照陈旧**：快照缺 selfheal/intents/rolling_summary/sales/business_card 等新文件，compose 误 `--build` 会整库回退——以容器权威码反向刷新宿主。
- **docker exec 默认非 root → 删除/写 addons 报 Permission denied**：须 `docker exec -u root`。
- **odoo shell 不带 `--no-http` 绑 8069 报 Address in use**；连接参数取自容器 entrypoint env。

## 五、权限/ACL 坑
- **engine 服务账号连 project.task 都无 unlink 权限**（search 可读）→ 删除类运维走容器内 superuser shell。
- **odooGenericUnlink 走 uid 身份先查 ir.model 被 ACL 拦**：CRUD 黑名单校验前置查询本身需要权限。
- **Odoo execute_kw 的 kwargs 独立键被静默忽略**（须放 args 第 7 位）→ 炸 comodel ACL 且难定位。
- **search_read 不显式传 fields**、**search_count/read_group 不触发 comodel ACL=聚合逃生通道**（是绕过工具隔离的隐患视角）。
- **无身份请求的假身份**：缺 `x-odoo-user` 时后端 defaultTier=business → 返回 sales 假身份，前端须不渲染卡片。
- **审批权限最终裁决在后端**：前端白名单只是渲染开关，后端 403 才是裁决；自测临时加的白名单行交付前必须删除。

## 六、"自动/自觉"能力不可靠（规则化兜底）坑
- **改码请求依赖模型自觉登记 → 会先反问细节而不登记**（2 次复现）→ 卡片随模型状态波动。解法：服务端确定性兜底 `enforceCodeChangeCard`（命中改码意图且本轮未登记 → 强制补登记卡片）。
- **RAG 端点族不升级 agent**：意图正则只在 agent 端点族生效，RAG 端点族命中不了就掉纯 RAG（答"知识库中没有相关内容"）。**必须测两个端点族**，不能只测 agent/chat。
- **导出/报价单意图掉纯 RAG**：新意图要加格式词约束（EXPORT_QUOTE_INTENT_RE 导出须带"导出成 Excel"这类格式词，防"电价配置怎么导出"被误拽）。

## 七、死循环/假执行坑
- **路径切片 off-by-one → 0xc000003a 全量死循环**（kb-sync 灌库）。
- **smbprotocol 1.17 用 scandir 不用 listdir_path**。
- **删 .bak 又自动备份出新 .bak（套娃）**：删除操作要有 `backup:false` 开关。
- **agent /read 对不存在文件返回 `cat: No such file...`**：engine 侧无法识别"文件不存在→新建"分支 → agent 归一化为 FILE_NOT_FOUND + 兜底正则。
- **`TOOL_RESULT_MAX=4000` 通用截断毁结构化结果**：读大文件返回 `{truncated:'<JSON残片>'}` 表现为 0 chars → 工具必须自带分页。
- **测试残余污染生产数据**：测试问句/导出问句会经 rememberQA 落库污染 agent_memory → 测完必清；测试变更单/测试文件必须清零。

## 八、前端竞态坑（跨端协作）
- **关流竞态导致气泡冻结**：agent 把整段答案作一次 delta 发出、紧接 done 并立即关流，前端打字机还没来得及播完即被 stopTyper 杀掉且 finishStream 因 donePending=true 不执行 → 刷新页面才正常。不能用"服务端延迟关流"的假修复（sleep 掩盖竞态，且非前端根因），须修前端 res.done 分支。

## 九、测试/验收假象坑（方法论）
- **探测脚本忘了身份参数**：不带 odoo_user 时 tryTaskIntent 守卫直接 return null（无身份不升级 agent 是设计内）→ 会得出"修复没生效"的假象。写探测脚本必须带真实身份、并了解守卫分支。
- **脚本在 A 容器内读 B 容器 bind mount 路径 → ENOENT 属预期**：读回应在目标容器内做或经 agent /read。
- **回归脚本要内置自清**，避免冒烟污染审批列表/数据库。
- **验收不靠推理下结论**：真实 HTTP 对话链路多组用例 × 双端点族实测。
- **指纹必须现场重算**：给外部文档的 md5 表沿用上轮会过时（config.js 已从一轮变到另一轮）。

## 十、编码协议坑位速查（Odoo/前端对接）
- `source` 参数必须 encodeURIComponent（中文/空格/括号截断 query）；参数名是 `source` 不是 `path`。
- HTTPS 页面禁直连内网 engine 服务（被混合内容拦截）→ 走同源代理。
- 改 Odoo JS 后须清 `ir.attachment` `/web/assets/%` 缓存并重启；改 .py 后 restart。
- OpenAI 兼容 `v1/chat/completions` 不传 tools 自动走 harness；agent 整段 delta 一次下发（无法真逐字，要逐字用 RAG 流式端点）。