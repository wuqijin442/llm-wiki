> 来源：WorkBuddy 记忆（已脱敏，隐去地址/账号/密码/Key/域名/人名）

---
title: DGX AI 智能体工程铁律
aliases:
  - DGX-AI工程铁律
  - AI智能体部署铁律
  - DGX RAG 与自愈闭环设计
tags:
  - AI智能体
  - 工程铁律
  - 部署
  - RAG
  - 自愈闭环
  - Ollama
  - Dify
  - Odoo
category: raw
created: 2026-09-12
updated: 2026-09-12
sources:
  - WorkBuddy sessions 2e03fae6 / a9cbf2d7 MEMORY.md
---

# DGX AI 智能体工程铁律（脱敏）

> 跨会话必记铁律合集，源自 DGX AI 智能体项目的长期工程记忆。

## 一、定位与协作边界
- 三工程师并行模型：A=统一账号/Gitea 仓库，B=前端（Trae/Odoo 前端），C=后端（Ollama/Dify/engine 联动，为整个系统主责）。各司其职，不越界改动他人模块。
- 给 B 的回执采用"唯一权威版"模式：只维护一份总回执，新增/修改契约只回写该份，旧文档归档并更新索引。避免多份文档冲突。

## 二、基础设施/部署铁律（⚠️ 最高优先）
- **镜像烤代码、无 bind mount**：部署一律 `scp → docker cp → restart → 容器内 grep/md5 校验`。不要相信陈旧快照（宿主源码快照常滞后于容器，compose 误重建会整库回退）。
- **compose 重建后 docker cp 归零**：容器代码是 docker cp 进去的，重建会清空；odoo-web 重建还需重装 openpyxl/pptx/opencc/libreoffice 等依赖（容器内未固化）。
- **反向漂移同样致命**：服务器侧改码必须回填本地仓库；改前必 diff 本地↔容器确认无漂移；验收=local↔container md5 一致。
- **部署链路固化为五步**：本地改码 → scp → docker cp → 容器校验(md5/语法) → 反向刷宿主快照。
- **⚠️ ssh 嵌套引号复合命令必炸**：复杂操作写本地脚本 scp 上去执行；**命令行明文传 key 会触发敏感审批卡死**——脚本内从容器 env 实时读 KEY。
- **别信命令回显**：容器内核对 key 用 `printf %s $API_KEY` 读真值再对照（输出层字符串替换会误导）。
- **本机 OpenSSH 故障绕行**：`C:/Windows/System32/OpenSSH/ssh.exe` 任何命令（含 `ssh -V`）都返回 255 且零输出——是进程级故障，与密钥/网络/HOME 无关。直接换用 `C:/Program Files/Git/usr/bin/ssh.exe` 即可。同故障期伴生：shim 报 `dirname/ls: command not found`（改走 PowerShell + 专用工具）、PowerShell 输出丢失、python 崩溃（node 正常）。
- **部署运行器**：封装 `ssh "cmd" / scp / run <local.sh> <dir>` 三种模式；全部走 spawnSync argv 不经本地 shell，并显式注入干净 PATH/HOME。注意 `engine-c/package.json` 是 `type:module`，运行器必须 `.cjs` 扩展名。
- 容器 exec 默认用户非 root，提权要 `-u root`；odoo-web 内无 curl，用 python3 urllib。
- 删除容器内 addons 文件必须 `docker exec -u root ... rm`（默认非 root 报 Permission denied）；本地 rm 在 shim 损坏时不可用 → 用 node `fs.unlinkSync`。

### docker.sock/socket 规避（自愈通道设计）
- engine-c-api 容器无 docker.sock/CLI：**不挂 sock、不重建容器**（保生产零中断）→ 在宿主跑极简执行 agent（`~/dgx-agent/dgx_agent.py`），监听本地环回端口（engine-c-api 是 host 网络可直连）。token 经 docker cp 注入容器内文件（root 600，容器进程 uid=0 可读），**不进 env/命令行**。
- 白名单双端校验：文件仅限指定路径前缀（如 `/mnt/extra-addons/ai_agent_integration/**`，禁 `..`）；重启仅白名单容器且**工具只暴露 odoo-web**（engine 自重启=进程自杀）。
- 写入走 base64 注入容器内 python 执行（避免引号地狱）；写前自动 `.bak_<ts>` 备份；删除是最高危操作 → `/delete` 端点仅供运维直调、不暴露为 LLM 工具，且需 `backup:false` 开关（否则删 .bak 会套娃出新 .bak）。
- crontab 每分钟保活；运维手册写成幂等安装+自检脚本。

## 三、Ollama / Dify 自托管
- Ollama systemd：NUM_PARALLEL=4、MAX_LOADED_MODELS=2、KEEP_ALIVE=30m。
- Dify 1.13 自托管：workflow 内调 Ollama 用网关地址（桥接网段），调 Service API 用容器间网络地址（本机桥接网段端口）。
- **必配 num_ctx=8192**（不传会 OOM）；同步大模型脚本禁 `--big`（72B 已删）。改挂模型后查 `apps JOIN app_model_configs`（model 列须 `::jsonb`）。
- compose 里补 `OLLAMA_NUM_CTX: '16384'` 原则：**只改文件不重建容器**（重建清 docker cp 代码），`docker compose config -q` 验证，下次重建自动生效。

## 四、RAG 与向量库架构
- **向量维度守恒**：现用库 1024 维（bge-m3）；旧 nomic 768 维仅 legacy。代码库与文档库分开，均 1024 维。
- **Qdrant 1.18.3 自构建坑**：无 `POST /points/upsert`（404），写入走老式 `PUT /collections/{c}/points?wait=false`；scroll/search 正常。
- **keyword 过滤但 0 命中**：必须 `match.value`，用 `match.text` 分词会假 0 命中。
- ⚠️ **JS 处理 Qdrant u64 id 丢精度**：失败列表里记录的 id 字面量≠真身，按 id 反查必 miss——补漏靠"超长文本候选集合级重跑"（>5000 字符截 3000 重嵌，同 id 覆盖幂等）。embed 超 bge-m3 上下文是成批失败的根因。
- **GraphRAG 评估定案：不引入**：bge-m3 + payload 过滤已覆盖主库；6900+ chunk 用 LLM 抽关系成本收益不成比例；代码检索对口方案=来源前缀嵌入 + 应用层过滤。
- 检索超参示例：answerModel=qwen3-coder:30b；num_predict 1024、timeout 180s；minScore=0.5、topK=8。慢在 LLM 生成而非检索。
- 原文件预览：宿主起代理进程（本地回环端口，crontab 保活）+ engine-c `GET /api/knowledge/file?source=`（24h 缓存 200MB）；source 须 `encodeURIComponent`。
- **Gitea 代码入库 Webhook 管道**：写 1024 维代码库；payload 兼容多种过滤；写前按 repo+filepath 删点幂等覆盖；嵌入前缀 `owner/repo/filepath\nchunk`；扩展名白名单 + 150KB 上限 + 单 push≤60 文件；Secret HMAC-SHA256 校验（secret 存容器内文件 root600，不进 env/compose——compose 重建归零坑）。
- Gitea 对接坑：raw 接口返回纯文本，`fetchJson` 会 JSON.parse 炸 → 用 `fetchWithTimeout + res.text()`；git trees API 键是 `tree`（非 entries）且 recursive 大仓要分页（page+total_count）。

## 五、Odoo AI Agent 工具集与权限
- **工具分档**：business / engineer / admin 三档硬隔离；admin 独占自愈类工具（code_editor / container_reloader），诊断类工具三档全员（输出按档裁剪）。工具数随版本演进（19→20→23，业务 19 / engine 10 / admin 23）。
- **Odoo 工具坑**：`project.task` 负责人是 `user_ids`（多对多）；state 未完成=`not in [1_done,1_canceled]`；**/jsonrpc execute_kw 的 kwargs 必须放 args 第 7 位**（独立键被静默忽略→炸 comodel ACL）；search_read 永远显式传 fields；search_count/read_group 不触发 comodel ACL=聚合逃生通道。
- **CRUD 权限面**：默认全模型放行 + 黑名单正则（`ir.*` / res.users.apikeys / res.users.log / auth_*）+ 敏感字段黑名单（password/credential/secret/token/api_key/oauth）永不开放；权限交给 record rule。黑名单正则必须拿真实敏感模型实测。
- **归属校验**：`/api/agent/files/:id` 文件名带 `u<uid>` 段 + 下载 403 拦他人文件；生成类工具全链透传 owner。
- **删除类运维操作**：工具身份及服务账号往往无 unlink 权限（search 可读）→ 走容器内 superuser shell（browse+unlink+commit）。
- **多公司约束**：Odoo `sale.order` 有 `_check_company`；客户/订单/明细必须同公司。把原生"绝密记录"类报错翻译成点名两家公司的可读信息。

## 六、harness 与 token 预算
- **numCtx 默认 16384**（硬限 16384~32768，**严禁 128K**，防并发 OOM）；**低于硬限会静默截断工具清单导致模型幻觉**（曾为 4096 → 模型凭空造工具名如 task_list / odoo_query）。
- maxSteps=6 硬停 + 软停止指纹。
- **Qwen3-Coder：消息以 role:'tool' 结尾会 500**，工具结果须以 user 消息回传。
- **每新增工具/上下文段都要核算 token 预算**（当前余量约一倍）。
- **Qwen3-Coder 只认首条 system**：runAgent 与 ragPrepare 只发一条 system，新增上下文段拼进同一条。
- ⚠️ **`TOOL_RESULT_MAX=4000` 通用截断会毁结构化结果**（超限返回 `{truncated:'<JSON残片>'}` 字段全丢）。**新增任何"读大内容"的工具必须自带分页**（from_line/to_line/total_lines/hint + offset 续读），永不超过 4000。code_editor 实测 11724B 文件曾被截成 0 可见字符，分页（2500 字符/120 行/次）后正常。
- 视觉模型：默认 qwen2.5vl:7b；历史根因=num_ctx=4096 被 ~4k token 挤爆 → 截断为空；qwen3-vl 是思考模型，num_predict 被 message.thinking 吃掉 → 正文空。现 num_ctx=16384 + num_predict=2048 + 空输出重试 + 日志。

## 七、意图路由与上下文装配
- **意图路由**：实体×动作+负向断言的正则升级 agent；粘性=正则 || stickyHit（8min Map，双键：会话键+账号键写入、任一命中即粘，因前端 B 首轮不带 session_id 二轮才带）；账号键命中须问句含实体词（防知识问句被拽进 agent），会话键无条件粘。
- ⚠️ **任何新意图都要考虑两个端点族**：RAG 端点族（`/api/rag_query_stream`）走 tryTaskIntent、命中不了正则就掉纯 RAG（无工具）。**别只测 `/api/agent/chat`**（直连 agent 端点无网关，一定过）。
- `/api/agent/chat` 入参是 `{messages:[...]}`，**不认 `{question}`**（会 400）；RAG 端点才用 `{question}`。写探测脚本时两个端点入参必须分开处理。
- **LLM 自觉不可靠 → 服务端确定性兜底**：改码请求模型可能先反问细节而不登记卡片（2 次复现）。在 agent_loop 出口加 `enforceCodeChangeCard`：命中改码意图且本轮无 create_change_request → 服务端强制补登记一张审批卡片（risk=medium，标注待补细节）。
- **上下文装配 payload**：`[偏好/pinned] + [task_anchor] + [滚动摘要] + [最近3轮原文]`；滚动摘要 >5 轮增量摘要、前缀哈希自愈、失败回退原文、摘要走并发池槽。
- 会话指纹=账号+session_id｜账号+前两条消息哈希（前端零改动生效）。
- 长对话排查先看响应 `contextInfo`（summaryChars/matches rounds）。

## 八、报价单 / 名片护栏
- **create_sale_order**：只产 draft 永不确认；⚠️ 纯数字 default_code 与 DB id 同形——先 default_code 再退化 id；多公司冲突须主动抛 COMPANY_MISMATCH；多义客户回 AMBIGUOUS + candidates 绝不自选；LLM 会把 lines 序列化成 Python repr → coerceLines 两轮容错；`create_customer_if_missing` 默认关闭（防重复 partner）。
- **parse_business_card**：凡结构化场景都要另写专用提取 prompt（通用描述实测会把邮箱识错）；查重=邮箱/手机/座机 OR domain；命中不重复建档；type 默认 lead 绝不自动转 opportunity；engineer 档硬拦；图片分支先落盘再识别，响应回 file_id。
- 工具在 executeTool 内跑（已在并发池槽内）→ 直接调 chat() 不绕过 OLLAMA_NUM_PARALLEL。

## 九、记忆与数据卫生
- Qdrant 记忆集合 1024 维，payload.user_id 隔离；GET/DELETE 带归属校验。
- **测试记忆问句会经 rememberQA 污染生产库**，测完必清（clean_memory.py）；测试导出/查询问句同样会落库。
- 历史脏数据：早前 agent 500 时落库的空 assistant 记录无害，提供清理 SQL。
- **LLM 大金额偶发数零**（如 4.5e9 答成 4.5 亿）——金额一律以返回体 `amount_total` 为准，不要采信 LLM 正文里的数字。

## 十、kb-sync 与灌库坑
- 灌库 cron 每日 04:30。坑①：smbprotocol 1.17 用 scandir 不用 listdir_path；坑②：路径切片 off-by-one → 0xc000003a 全量死循环（假成功）。
- **Edit 工具会假成功不落盘**（实测多次，尤其 import、新增函数定义）→ 改完必须 grep 复核 + node --check 再部署，**不能只 grep 调用处**。

## 十一、待办方向（等外部输入，非代码问题）
- WP 相关：站点 URL/用户/应用密码待外部注入 env，未配置时探针如实报"未配置"不静默跳过。
- `engineerGroups` 组名待对齐（现按 uid 白名单兜底生效）。
- 自愈边界：engine 自重启走人工（避免进程自杀）；LLM 全文重写上限约 500 行（超长须走 file_diffs 人工审阅）。