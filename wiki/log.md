# Wiki Log

> 操作日志（时间导向记录）。每次操作追加一条，最近 30 天在此展示，更早记录归档至 `wiki/logs/`。

## [2026-09-16] maintain | 自动化任务同步至 v2.1 / Proteus 式 episode 协议（用户指令）

- 触发：用户指令「参考 Proteus 升级项目并更新自动化任务内容，然后推送最新代码」
- 动作：automation_update 将 `llmWiki 每日素材自动入库`（bd9582d1）prompt 重写为 **observe→propose→act→reflect** 四相 episode 循环，对齐 AGENTS.md v2.1：
  - observe：差异检测 + 读 §8 活跃 disposition / GoalConfig（默认 no-goal）
  - propose：强制先列方案清单再 act
  - act：增量归并 + 熵减
  - reflect：记 churn/travel + 结晶判读 + 比较前可靠性门槛
- 配套：本条目与 09-16 Proteus 仓库精读 + Schema v2.1 升级**一并提交推送**（此前均本地未提交、未 push）
- 备注：自动化 prompt 存于 WorkBuddy automation store（非仓库文件）；本次推送的是 09-16 的仓库改动（AGENTS v2.1 / 新 Proteus 三页 / growth·log·index·SKILL·PROMPTS·README 更新）

## [2026-09-16] maintain | Schema v2.1：O-P-A-R / 变动分解 / disposition 协议 / 比较可靠性（用户已确认）

- 触发：Proteus 精读后的机制升级，用户在方案 A/B/C/D 中全选确认
- `AGENTS.md` → **v2.1**：§4 增补变动分解（added/revised/merged + churn + travel）；§7 Ingest 协议化为 observe→propose→act→reflect（reflect 必记意外）；§7 Lint 增补「比较前可靠性」；**新增 §8 可移除 disposition 实验协议**（单一/有期限/撤除后结晶判读）
- `wiki/growth.md`：复盘表增加 churn 列（历史行已回算）；累计 travel=45（至 09-16）；表头注明读法
- `PROMPTS.md`：ingest/lint/生长复盘提示词对齐 v2.1；新增 §9 disposition 实验模板；速查表 +1 行
- `skills/llm-wiki/SKILL.md`：核心机制节对齐 O-P-A-R、churn、§8
- `README.md`：工作流指令表同步（ingest/lint/生长复盘 + disposition 实验）
- 原则：**raw/ 零改动**；知识页正文不写入实验遵守率（防污染）
- 备注：未改 `.obsidian/`、未构建、未提交；与同日 Proteus ingest 条目配套

## [2026-09-16] ingest | Proteus 仓库精读（harness 自进化框架 + 测量标尺）

- 来源：`raw/articles/2026-09-16-Proteus-自进化harness框架-仓库精读`（浅克隆 github.com/proteus-evolve/Proteus @ 962304b，2026-08-28 提交；精读 README + EPISODE.md + MEASUREMENTS.md + disposition/episode_protocol/distance/crystallize/stream 源码；raw 既有文件零改动）
- 差异检测：raw/ 知识素材 18 篇全部已被引用；本次新建精读素材 = 唯一新增未消化素材
- 新建：[[Proteus-自进化harness框架-摘要]]（摘要页：三差异化主张、可迁移机制判断表、待核实 3 项）
- 新建：[[Proteus]]（实体页：定位/属性/架构分工/五条设计要点/CLI）
- 新建：[[Harness自进化]]（概念页：演化对象从权重到 harness；Surface/四相位/事务契约/认识论协议；与训练、自愈、LLM Wiki、RSI 的边界）
- 新建：[[自进化测量标尺]]（概念页：结构距离+churn+travel、行为距离+R、结晶两阶段；通用统计纪律十条；迁移到本库指标的场景）
- 更新：[[Agent]]（补「趋势观察（2026-09-16）」+ sources + 出链；RSI 议题落到可复现实验仪器；status 维持 growing）
- 更新：[[双环知识飞轮]]（补「对照：harness 自进化」小节 + Proteus 出入链；sources +1 → status seed→growing）
- 更新：[[AI智能体工程方法论]]（补 §6 Harness 演化的测量纪律 + sources；与「证据优先」文化挂钩）
- 更新：[[自生长知识库实战-苍何]]（关联页 +1 指向 [[自进化测量标尺]]；不计第二来源，status 维持 seed）
- 台账：index.md（32→36 页，素材 18→19）、log.md、growth.md、lifecycle.md
- 待核实：R=1.63→0.93 与 v0.3.0 清单为 README/快照自述，未对照论文；dsh/pi 构建细节未实跑；机制升级方案另案待确认（见 log 同日后续条目）
- 备注：未改 `.obsidian/`、未构建、未提交；**AGENTS.md Schema 变更待确认**（参考既有惯例）

## [2026-09-15] ingest | 每日AI素材-2026-09-15（四渠道头部采集 + 增量入库）

- 来源：`raw/00-Inbox/每日AI素材-2026-09-15`（本自动化第 0 步采集产出；GitHub Trending / Gitee LLM / CSDN 热榜 / arXiv cs.AI + WebSearch 热点补充；raw 既有文件零改动）
- 差异检测：raw/ 知识素材共 18 篇（排除 `80-Templates` 与 `articles/README.md` 基础设施文档）；17 篇已被引用，本次新建每日素材文件 = 唯一新增未消化素材
- 新建：[[每日AI素材-2026-09-15-摘要]]（摘要页：四渠道亮点提炼 + agent 工具化延续冲榜 + RSI/递归自我改进热点；含待核实 3 项）
- 更新：[[Agent]]（补「趋势观察（2026-09-15）」+ sources + 出链/入链；updated→2026-09-15，status 维持 growing）
- 台账：index.md（31→32 页，素材 17→18）、log.md、growth.md、lifecycle.md
- 待核实：RSIAgent「超过 GPT-6 Astra」数值来自新浪财经媒体报道，未对照论文原文；GitHub Trending「今日新增 Star」为页面声称值未独立核验；Gitee 列表为存量 Star 热度非当日趋势
- 备注：未改 `.obsidian/`、未构建、未提交、未改 AGENTS.md

## [2026-09-14] ingest | 每日AI素材-2026-09-14（四渠道头部采集 + 增量入库）

- 来源：`raw/00-Inbox/每日AI素材-2026-09-14`（本自动化第 0 步采集产出；GitHub Trending / Gitee LLM / CSDN 热榜 / arXiv cs.AI + WebSearch 热点补充；raw 既有文件零改动）
- 差异检测：raw/ 知识素材共 16 篇全部已被引用（排除 `80-Templates` 与 `articles/README.md` 基础设施文档）；本次新建每日素材文件 = 唯一新增未消化素材
- 新建：[[每日AI素材-2026-09-14-摘要]]（摘要页：四渠道亮点提炼 + agent 工具化冲榜趋势；含待核实 3 项）
- 更新：[[Agent]]（补「趋势观察（2026-09-14）」+ sources + 出链/入链；updated→2026-09-14，status 维持 growing）
- 台账：index.md（30→31 页，素材 16→17）、log.md、growth.md
- 待核实：GPT-6 Astra / Breaking the Token Ceiling 基准数值与「未发布权重」批评取自媒体与第三方博客，未对照原文；GitHub Trending 部分中间数值含义不明，仅采用明确标注的 stars today
- 备注：未改 `.obsidian/`、未构建、未提交、未改 AGENTS.md

## [2026-09-14] ingest | 双环知识飞轮（掘金）+ 每日素材源扩展

- 来源：`raw/articles/2026-07-28-你的知识库在自增长但那是AI的飞轮不是你的`（掘金 @默默65；**本次新增剪藏，raw 既有文件零改动**）
- 新建：[[双环知识飞轮-摘要]]（摘要页，含待核实 5 项）
- 新建：[[双环知识飞轮]]（概念页：编译环 / 互动环 / 回流带；harvest.py 三动作「抓-炼-写」；定时必须系统级；复利=连）
- 新建：[[ICAP学习框架]]（概念页：P/A/C/I 四层，`I > C > A > P`；费曼法只到 C；AI 把「圈内人」变随叫随到）
- 更新：[[LLM-Wiki-vs-RAG]]（补「存下来的是孤岛，编译过的是网络」+「编译是 AI 的建构，不等于人的学习」；sources +1 → `growing`）
- 更新：[[双链网络]]（补「复利的本质不是多，是连」+ 能力错觉的反面提醒；sources +1 → `growing`）
- 更新：[[自生长知识库实战-苍何]]（补「本文的边界：只覆盖编译环」+ 3 条关联入链）
- 变更（自动化）：`llmWiki 每日素材自动入库`（id `bd9582d1`）prompt 新增「第 0 步 · 每日素材采集」——CSDN 热榜 / GitHub Trending / Gitee `explore/llm` / arXiv `cs.AI` 四源，落 `raw/00-Inbox/每日AI素材-YYYY-MM-DD.md` 后进入原有 ingest 流程；四个源均已实测可抓取
- 台账：index.md（27→30 页，素材 15→16）、log.md、growth.md、lifecycle.md
- 待核实：ICAP 原始论文未核（二手转述）；「九成自增长库只转半圈」属主观判断；本库若落地「回流带」，会话来源 / 提炼标准 / 去重规则尚未定义
- 备注：**本次未改 `AGENTS.md`（Schema 变更待确认）**、未改 `.obsidian/`、未构建、未提交

## [2026-09-13] ingest | 无新增素材（自动化每日入库）

- 差异检测：raw/ 共 15 篇（跳过 `raw/80-Templates`）；14 篇知识素材已全部被 wiki 页 `sources` 引用；`raw/articles/README.md` 为剪藏目录配置说明，属基础设施文档，不计入知识素材
- 结论：**未消化素材 = 0**；raw/ 各文件 mtime 均 ≤ 2026-09-12 09:04，无新增、无修改
- 动作：本次 ingest 走空集分支——不新建、不补充任何页面，不凭空造内容
- 留痕：仅追加本记录（`wiki/log.md`），wiki/ 无内容页 diff
- 只读原则：raw/ 未做任何写操作

## [2026-09-12] ingest | 跨平台 VPN 客户端研发与合规（自动化每日入库）

- 来源：`raw/20-Tech/记忆-跨平台VPN客户端研发与合规`（WorkBuddy / Trae 记忆，已脱敏）
- 差异检测：raw/ 共 15 篇（跳过 80-Templates），本次仅 1 篇未消化；`raw/articles/README.md` 为剪藏目录配置说明，属基础设施文档，不计入知识素材
- 新建：[[跨平台VPN客户端研发与合规-摘要]]（摘要页，含待核实 3 项）
- 新建：[[混合跨平台客户端架构]]（概念页：Lit + Cordova/Electron/Xcode 打包矩阵、transportConfigLocation 统一下发、字节计数证据法）
- 新建：[[iOS内部分发与App Store合规]]（概念页：Unlisted 四件套、4.3.a / 5.1.x 拒审类别、签名一致性核对项）
- 更新：[[个人AI工程方法论]]（新增「双版本汇报」小节；证据优先补「可复验抓包口径」；sources +1 → status: seed → growing）
- 更新：index.md、log.md、growth.md（复盘表 +1 行）、lifecycle.md（复核台账 +1 行）
- 待核实：ITMS-91054 与「重复内容」对应关系未对照 Apple 官方文档验证；拒审条款细节素材仅给类别

## [2026-09-12] maintain | status 回填 + Dataview 安装 + 自动化配置

- 回填：24 个内容页 frontmatter 补 `status`（20 `seed` / 4 `growing`）；规则 = `sources` ≥2 记 `growing`，否则 `seed`；逐字节保留 LF，换行零漂移
- 修正：`wiki/index.md` 计数 → 内容页 24、素材 14
- 启用：安装社区插件 **Dataview 0.5.70**（main.js 2,377,634 B / manifest.json 357 B / styles.css 2,965 B，取自官方 release assets 并校验）
- 修正：`.obsidian/community-plugins.json` 原为 `{ "community-plugins": [...], "plugins": [] }` 对象结构——Obsidian 只认字符串数组，**原格式等于插件从未被启用**；已改为 `["dataview"]`
- 配置：2 条自动化 ——「llmWiki 每日素材自动入库」（每天 21:00，cwd = vault）、「llmWiki 每周健康检查 lint」（每周一 09:00）
- 台账：`growth.md` 生长复盘表与 `lifecycle.md` 复核台账各追加一行
- 备注：自动化不支持在 API 侧绑定模型，需将 WorkBuddy 默认对话模型设为「混元 Hy3」

## [2026-09-12] ingest | 自生长知识库实战（苍何）— 搭建后首次验证
- 来源：`raw/articles/2026-09-12-用WorkBuddy-Codex-Obsidian搭建自生长知识库.md`
- 新建：[[自生长知识库实战-苍何]]（摘要页，含三层架构、三种搭建路径对比、待核实 3 项）
- 更新：[[Obsidian-LLM-Wiki实操指南-摘要]]（补入同主题交叉引用）
- 更新：index.md、log.md
- 说明：本次为搭建完成后的验证性 ingest，用于确认「素材入 raw → wiki 出现增量」的闭环可用；已有页面 [[Obsidian]] / [[RAG]] / [[LLM-Wiki-vs-RAG]] / [[双链网络]] 均命中复用，未重复造页

## [2026-09-12] build | 自生长机制 v2 搭建（对齐 LLM Wiki 方法论）
- 参考：《用 WorkBuddy / Codex + Obsidian 搭建自生长的个人知识库实战》
- 升级：`AGENTS.md` → v2（新增 §4 自生长机制、§5 冲突与不确定性处理、§6 页面生命周期；frontmatter 增补 `status` / `confidence` / `scope`；Ingest 增补差异检测与增量归并，Lint 增补 5 项检查）
- 新建：[[growth]]（🌱 生长看板，9 组 Dataview 指标）、[[lifecycle]]（🔄 生命周期台账）
- 新建：`PROMPTS.md`（操作提示词手册，8 组可直接复制的提示词）
- 新建：`raw/articles/`（网页剪藏落地目录 + Web Clipper 配置说明）
- 补齐目录：`raw/01-Daily`、`raw/30-Life`、`raw/90-Attachments`、`raw/95-Archive`、`wiki/logs`、`output/{posts,reports,slides,tutorials,newsletters}`
- 升级：`skills/llm-wiki/SKILL.md` → v2；5 个页面模板增补 `status`/`confidence`/`scope`，摘要模板「疑问待确认」统一为「待核实」
- 更新：`README.md`（三层架构说明 + 三种接入方式对照）、[[仪表盘]]（统一文件夹过滤 + 状态分布）
- 修正：index.md 页面总数 22 → 23（原计数与实际页面数不符）
- 待办：现有 23 个内容页尚未回填 `status`，见 [[growth]] §6 与 [[lifecycle]] §6

## [2026-09-12] ingest | WorkBuddy 全工作区记忆补充（脱敏提炼）
- 操作：补读 WorkBuddy 会话记忆文档（`workspace/sessions/*/modify_backup/` 下的 MEMORY.md ×2、按日日志若干、agent_system.md、工程师回执），补全此前遗漏的工作区记忆
- 来源（脱敏素材，见 raw/20-Tech）：记忆-DGX-AI智能体工程铁律 / 记忆-DGX-工作区日志与回执要点 / 记忆-DGX-已知坑位与疏漏教训集
- 新建：[[自愈闭环设计]]、[[意图路由与服务端兜底]]（概念页）
- 新建：[[AI智能体工程方法论]]（综合分析页）
- 更新：index.md、log.md
- 备注：明文 IP/账号/密码/Key/域名/人名已全部脱敏；原始 10MB+ jsonl 会话转储仅作背景，未逐条入库

## [2026-09-12] ingest | 本地 WorkBuddy / Trae 记忆（脱敏提炼）
- 操作：读取本机 WorkBuddy 记忆档案 + Trae 记忆，**脱敏**（剔除密码/账号/内网地址/客户名后）提炼为 5 篇知识素材
- 来源（脱敏素材，见 raw/20-Tech 下 `记忆-*` 5 篇）
- 新建：[[AI智能体分权治理]]、[[显示与存储单位分离]]、[[本地优先AI创作]]（概念页）
- 新建：[[多租户架构]]、[[Ollama]]、[[ComfyUI]]（实体页）
- 新建：[[个人AI工程方法论]]（综合分析页）
- 新建：[[个人协作偏好与工程方法论-摘要]]（摘要页）
- 更新：index.md、log.md
- 备注：WorkBuddy 原始 jsonl（agent 转储 10MB+）仅作背景参考，未全文入库以降低噪音与敏感面；保密凭据未进入知识库

## [2026-09-12] ingest | AI大模型应用学习笔记（批量 4 篇）
- 来源：`raw/20-Tech/RAG检索增强生成.md`
- 新建：[[RAG检索增强生成学习笔记]]（摘要页）、[[Milvus]]（实体页）
- 更新：[[RAG]]（新增检索优化与记忆层章节、sources）
- 来源：`raw/20-Tech/为什么转岗AI大模型应用.md`
- 新建：[[为什么转岗AI大模型应用]]（摘要页）、[[AI应用开发]]、[[Agent]]（概念页）
- 来源：`raw/10-Work/langgenius-dify.md`
- 新建：[[Dify]]（实体页）
- 来源：`raw/10-Work/GitHub-AI周报-2026-08-09.md`
- 新建：[[GitHub-AI周报2026-08-09]]（摘要页）
- 更新：index.md、log.md

## [2026-09-12] ingest | Obsidian + LLM Wiki 实操指南
- 来源：`raw/00-Inbox/Obsidian-LLM-Wiki实操指南.md`
- 新建：[[Obsidian-LLM-Wiki实操指南-摘要]]（摘要页）
- 新建：[[Obsidian]]（实体页）
- 新建：[[RAG]]、[[双链网络]]（概念页）
- 新建：[[LLM-Wiki-vs-RAG]]（对比分析页）
- 更新：index.md、log.md

## [2026-09-12] init | 初始化知识库

- 操作：搭建四层目录结构（raw / wiki / output / skills）
- 新建：`AGENTS.md`（Schema 规范）
- 新建：`skills/llm-wiki/SKILL.md`（Skill 工作流）
- 新建：`raw/80-Templates/` 下 6 个笔记模板
- 新建：`wiki/index.md`、`wiki/log.md`
- 配置：Obsidian 附件路径、每日笔记、模板插件
- 新建：`README.md`（使用说明）

---

## 日志格式参考

### [YYYY-MM-DD] ingest | 素材标题
- 来源：`raw/路径/文件名`
- 新建：[[页面1]]、[[页面2]]
- 更新：[[页面3]]（新增xxx章节）
- 更新：index.md

### [YYYY-MM-DD] query | 查询问题
- 查阅：[[页面1]]、[[页面2]]
- 产出：[[对比分析页]]（已存入 wiki）

### [YYYY-MM-DD] lint | 健康检查
- 发现矛盾：[[页面A]] 与 [[页面B]] 关于xxx的描述不一致
- 孤立页面：[[页面C]]（无入链）
- 缺失概念页：建议创建 [[概念X]]
- 已修复：N 项 | 待修复：M 项

### [YYYY-MM-DD] publish | 成品标题
- 类型：post | report | slides | tutorial | newsletter
- 输出：`output/posts/文件名.md`
- 来源：[[wiki 页面1]]、[[wiki 页面2]]...
- 状态：草稿 | 已审核