---
title: Refresh 核验记录 2026-09-22
date: 2026-09-22
type: refresh-verify
sources:
  - "[[raw/00-Inbox/每日AI素材-2026-09-21]]"
description: 对 09-21 入库三处待核实项（ZCode 外泄 / SEP-2640 / Paper2Agent）联网对照一手来源的核验结论。
status: growing
---

# Refresh 核验记录 2026-09-22

> 对照对象：[[每日AI素材-2026-09-21-摘要]] 中标记的三处待核实项。核验时间 2026-09-22，一手来源见各条链接。

## 1. ZCode 数据外泄事件（Agent 概念页 · 已核实 + 已闭环）

**结论：真实事件，已从「威胁模型」升级为「已整改闭环案例」。多源交叉确认，非单源 researcher 报告。**

- 曝光（2026-09-18）：开发者 ferstar 逆向发现，智谱 ZCode 在登录态静默打包整个工作区（含完整 `.git` 历史 / LFS 缓存 / reflog / 已删未推配置）加密上传阿里云 OSS（域名 `zcode.z.ai`、`cdn-zcode.z.ai`）；RSA 公钥由服务端下发、私钥仅存云端，本地不可解；UI 两开关（体验优化 / 仓库快照索引）均无法关闭静默上传；313MB 包连续失败上传 564 次滞留本地。
  - 来源：[钱江晚报/潮新闻](https://www.toutiao.com/article/7687126743274013199/) / [搜狐科技](https://www.163.com/dy/article/L7CJ4RB305508TBC.html)
- 智谱 9-18 致歉：称源于默认开启的「代码库索引」功能（用于生成 Repo Wiki + 检查点恢复），数据生成 Wiki 后销毁。
- 企业用户发函（2026-09-19）：太原承明科技自查 8/28–9/14 超 6 工作区被上传，最大 391.94MB，含源码 / 已删提交 / 数据库口令 / 接口密钥 / 云凭证 / 员工用户信息；数据流向指向新加坡主体 **JINGSHENG HENGXING TECHNOLOGY PTE. LTD**。
  - 来源：[腾讯新闻](https://news.qq.com/rain/a/20260921A05N8500)
- 整改闭环（2026-09-21）：智谱宣布 **开源 ZCode**，`v3.14.0` 移除 Repo Wiki 及本地仓库快照上传链路；**中国信通院 + 绿盟科技**审计确认 `zcode-prod` OSS 存储桶及全部数据对象已删除（云端零数据）；MaaS 上线「数据内容不留存」；创始人唐杰亲自下场投诉相关用户。
  - 来源：[中国新闻网/今日头条](https://www.toutiao.com/article/7687874189411369482) / [The Standard (HK)](https://www.thestandard.com.hk/innovation/article/343354/ZAI-open-sources-ZCode-after-coding-tool-uploads-local-data-silently)

**对知识库含义**：强化 [[意图路由与服务端兜底]]「出口需显式核算」——agent 出口默认常开 + 不可关是比模型更危险的失败面。

## 2. SEP-2640 Skills over MCP（Harness自进化 概念页 · 已核实 + 补强 caveat）

**结论：Final 状态经 MCP 官网核实为真；新增关键 caveat——「Final」仅协议层定稿，客户端尚不可用。**

- 官方 SEP 页：[modelcontextprotocol.io/seps/2640-skills-extension](https://modelcontextprotocol.io/seps/2640-skills-extension) — Status **Final**，基于现有 **Resources primitive** 暴露 Agent Skills，约定 `skill://` URI scheme，定义 `skills/list` + `skills/get` 两方法；由 Skills Over MCP Working Group 维护；协议版本 ≥ 2026-07-28；Created 2026-04-23。
- 扩展总览：[modelcontextprotocol.io/extensions/skills/overview](https://modelcontextprotocol.io/extensions/skills/overview) — `skill://code-review/SKILL.md` 形式，`frontmatter` 含 name/description，`resources` 带 SHA-256 digest + size；读取 SKILL.md 不自动激活 skill，需 host 经 skill-loading 路径校验 + 用户批准。
- **关键 caveat（修正 09-21 表述）**：规范 9/13 合入 Final，但官方 **Go / Python / TypeScript / C# SDK 的 Skills 支持 PR 仍 open**，客户端尚不能端到端消费——「Final」= 协议稳定 ≠ 客户端可用。设计刻意复用 Resources 面、不新增协议面。
  - 来源：[MindPattern 9/18 分析](https://mindpattern.ai/s/2026-09-18-mcp-s-skills-extension-is-final-and-no-sdk-speaks-it-end-to-end-yet) / [MindPattern 合入记录](https://mindpattern.ai/f/25255) / [AGI Hunt](https://agihunt.info/p/1a0b259aa3b144aed07a390cbc5)

**对知识库含义**：本库 `skills/` 目录 + `SKILL.md` frontmatter 格式与 SEP-2640 同构（本库 skill 格式超前于协议）；`skill://` 协议化使 skill surface 治理从「项目内约定」外溢到「协议层供应链」。

## 3. Paper2Agent 论文即可执行（RAG 概念页 · 已核实，数字确认）

**结论：Nature 一手来源 + 多个二级来源交叉确认 09-21 数字；补充作者/DOI/失败模式。**

- 一手来源：[Nature 新闻](https://www.nature.com/articles/d41586-026-02899-2) + [Nature 论文 DOI 10.1038/s41586-026-11044-y](https://www.nature.com/articles/s41586-026-11044-y)；作者 **Jiacheng Miao, Joe R. Davis, Yaohui Zhang, Jonathan K. Pritchard, James Zou**（Stanford），刊 2026-09-16。
- 机制：把论文（手稿 / 代码 / 数据 / 工作流）转成 **MCP 服务器** + 多 agent 构建工具，形成「虚拟通讯作者」（virtual corresponding author）；6 步流程 = codebase 识别 → 环境搭建 → tutorial 发现 → 执行 → 工具提取 → MCP 组装；工具经参考代码库结果校验后锁定部署（防 LLM 代码幻觉）。
- 规模数字（已确认）：**100 篇计算生物论文 → 74 篇做成可用 agent**，599 工具提议、**593 通过自动校验**。
- AlphaGenome showcase：22 工具、~45 min、$14；tutorial-derived 查询 **98.7%**（vs Claude+Repo 82.7%、Biomni 37.3%），novel 查询 **100%**（vs 78.7% / 56.0%）；跨 300 题 ensemble 91.2%。
- 多 agent 协作：AlphaGenome + MPRA-scCRISPRi + Perturb-seq 链锁定 **GPR137** 为银屑病变异 rs887314 可能因果基因（Spearman 0.613）。
  - 二级来源：[AI Weekly](https://aiweekly.co/alerts/paper2agent-turns-nature-papers-into-interactive-ai-agents) / [Complete AI Training](https://completeaitraining.com/news/researchers-turn-scientific-papers-into-ai-agents-that)
- **caveat（补强）**：失败主因 = 缺可执行代码 / 缺数据或模型工件 / 环境失败 / 脚本不可泛化；论文未报告失败原因；「执行通过验证 ≠ 科学结论正确」，假设提出仍须人类主导。

**对知识库含义**：与 [[双环知识飞轮]] / [[自生长知识库实战-苍何]]「知识库自生长」同源，但 Paper2Agent 是论文级（方法→可执行工具）、LLM Wiki 是库级（素材→结构化页面）。

## 未在本轮核验的项（范围纪律，留待下轮）

- CodeMidas / GraphSkillEvo / DisCo 数值（页面声称值，未逐条对照 arXiv 原文）。
- harness 176 设定消融（源自 Daily AI Brief 转述，非一手实验）。
- GitHub / Gitee / arXiv 当日 Star / 编号（页面声称值，趋势采集固有属性，不独立核验）。
