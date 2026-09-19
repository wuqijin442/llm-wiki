# Wiki Log

> 操作日志（时间导向记录）。每次操作追加一条，最近 30 天在此展示，更早记录归档至 `wiki/logs/`。

## [2026-09-19 晚] ingest | 每日AI素材-2026-09-19-晚（四渠道头部采集 + 增量入库，同日第 2 批）

- 来源：`raw/00-Inbox/每日AI素材-2026-09-19-晚.md`（本自动化第 0 步采集产出；同日晚间第 2 批；GitHub Trending / Gitee LLM / CSDN 热榜 / arXiv cs.AI + WebSearch 当日热点补充；raw 既有文件零改动）
- 差异检测：raw/ 知识素材共 23 篇（排除 `80-Templates` 与 `articles/README.md`）；22 篇已被引用，本次新建晚间每日素材文件 = 唯一新增未消化素材（待消化集合 = 1）。注：同日早间 `每日AI素材-2026-09-19.md` 已入库，晚间批次为同日第 2 批，落 `每日AI素材-2026-09-19-晚.md` 以避免覆盖已消化文件、遵守 raw 只读纪律
- 新建：[[每日AI素材-2026-09-19-晚-摘要]]（摘要页：五大信号——harness 成本被单独度量（HarnessTax 实证，同一模型三套 harness 成功率不变、成本差最高 5x）、coding-agent/skills 生态延续霸榜 + AGENTS.md 规范化、微型/边缘端自动化基础模型（needle/Needle 3/Edge0）、多模态/视频推理优化（vLLM+NVIDIA/VAE/FPS）、AI 治理合规升温（加州 kill switch/弗州放缓/三巨头自监管/苹果防伪）；含待核实 7 项）
- 更新：[[Agent]]（frontmatter +1 source；新增「每日素材信号（2026-09-19-晚）」H3 子节，沿用 H3 规避重复 H2；相关链接 +1；updated→2026-09-19，status 维持 growing）
- 更新：[[Harness自进化]]（frontmatter +1 source；「与相关概念的边界」节补「外部印证（2026-09-19-晚）」——HarnessTax + arXiv 2609.20474 量化 harness 价值 + byobot harness tax 概念，把「演化对象=harness」从组件级科学化推进到成本侧度量；相关链接 +1；updated→2026-09-19，status 维持 seed）
- 台账：index.md（39→40 页、素材 22→23）、log.md、growth.md、lifecycle.md
- Propose 清单（本次遵守）：新建摘要页 1 + 增量归并 Agent.md + Harness自进化.md；§8 当前无活跃 disposition（默认 no-goal 自由生长），不强行造内容；已识别冲突点=Agent.md 已有 09-14/15/16 趋势节与 09-16/18/19 信号 H3 → 新增 09-19-晚 信号沿用 H3 子节规避重复 H2
- 意外与未解问题：① CSDN hot-rank 接口本轮仍仅返回 nickName+viewCount（标题/链接/hotValue 全缺失），WebSearch 按作者+关键词补标题未命中（返回其历史博客非本日热榜）→ 按约定降级记「标题未取到」，CSDN 渠道本日信息量受限；② GitHub/Gitee/arXiv 列表与早间同口径（GitHub「今日 Star」为当日累计、Gitee 为存量热度、arXiv 仍为 09-18 批次），本轮真正新增信号来自 WebSearch 当日热点；③ 同日第 2 批致素材文件名带 `-晚` 后缀，索引与摘要页已显式标注「同日第 2 批」防歧义
- 待核实：GitHub/Gitee/arXiv 数值为页面声称值未独立核验；HarnessTax 为 BYOBot 媒体转述未对照原始论文与基准数据；GLM-5.3-Flash/Qwen3.8 Omni Flash 为媒体口径未对照官方；加州/弗州 AI 行政令为新闻报道未对照政府原文；Cactus Needle 3/AutoArk Edge0 延迟吞吐数字来自 AGI HUNT 日报未对照仓库 README 或基准；CSDN 标题未取到
- 备注：未改 `.obsidian/`、未构建、未提交（入库后由 step 6 自动 commit + push）

## [2026-09-19] ingest | 每日AI素材-2026-09-19（四渠道头部采集 + 增量入库）

- 来源：`raw/00-Inbox/每日AI素材-2026-09-19.md`（本自动化第 0 步采集产出；GitHub Trending / Gitee LLM / CSDN 热榜 / arXiv cs.AI + WebSearch 热点补充；raw 既有文件零改动）
- 差异检测：raw/ 知识素材共 22 篇（排除 `80-Templates` 与 `articles/README.md`）；21 篇已被引用，本次新建每日素材文件 = 唯一新增未消化素材（待消化集合 = 1）
- 新建：[[每日AI素材-2026-09-19-摘要]]（摘要页：三大信号——harness 成为组件级研究对象（arXiv 密集群：2609.20804 组件级实证 / SoL-Pi / Harness Value + EurekAgent 环境工程）、skill 生态走向安全审计化（security-audit-skill 当日头名 + SkillAA 回滚）、自维护 Wiki 第三次外部印证（LangChain OpenWiki：WeKnora→Agents-Flex→OpenWiki 证据链）；含待核实 8 项）
- 更新：[[Agent]]（frontmatter +1 source；新增「每日素材信号（2026-09-19）」H3 子节，沿用 H3 规避重复 H2；相关链接 +1；updated→2026-09-19，status 维持 growing）
- 更新：[[Harness自进化]]（「与相关概念的边界」节补「外部印证（2026-09-19）」——harness 组件级实证三篇 + EurekAgent + SkillAA 事务化 + OpenWiki 第三印证；相关链接 +1；updated→2026-09-19，status 维持 seed）
- 台账：index.md（38→39 页、素材 21→22）、log.md、growth.md、lifecycle.md
- Propose 清单（本次遵守）：新建摘要页 1 + 增量归并 Agent.md + Harness自进化.md（外部印证）；§8 当前无活跃 disposition（默认 no-goal 自由生长），不强行造内容；已识别冲突点=Agent.md 已有 09-14/15 趋势 H2 与 09-16/18 信号 H3 → 新增 09-19 信号沿用 H3 子节规避重复 H2
- 意外与未解问题：① CSDN hot-rank 接口本轮仅返回 nickName+viewCount（标题/链接/hotValue 全缺失），WebSearch 补标题未命中 → 按约定降级记「标题未取到」，CSDN 渠道本日信息量受限；② ECC 总 Star 262K 存疑连续第三日未消除，建议下轮 refresh 用 GitHub API 核实真实值；③ OpenWiki 两篇仅标题级信息，正文未读，落地深度待核
- 待核实：arXiv 2609.xxxxx 编号/标题未逐条核验；HuggingFace upvotes 来自第三方博客未对照；Atria Dawn Preview 744B/40B/MIT 数值来自 miraflow.ai 日报未对照论文；EurekAgent <$11、Ask the Tool TTFT p90 -20.7% 未对照原文；GitHub stars today 为页面声称值（ECC 262K 存疑）；Gitee 为存量热度口径非当日趋势；OpenWiki 案例正文细节未读
- 备注：未改 `.obsidian/`、未构建、未提交（入库后由 step 6 自动 commit + push）

## [2026-09-18] ingest | 每日AI素材-2026-09-18（四渠道头部采集 + 增量入库）

- 来源：`raw/00-Inbox/每日AI素材-2026-09-18.md`（本自动化第 0 步采集产出；GitHub Trending / Gitee LLM / CSDN 热榜 / arXiv cs.AI + WebSearch 热点补充；HuggingFace papers 本环境 fetch failed → 改用 WebSearch 补 5 条；raw 既有文件零改动）
- 差异检测：raw/ 知识素材共 21 篇（排除 `80-Templates` 与 `articles/README.md`）；20 篇已被引用，本次新建每日素材文件 = 唯一新增未消化素材（经核对 `每日AI素材-2026-09-14/15/16` 均已引用、`2026-09-18` 尚未引用，确认待消化集合 = 1）。**注：09-17 无采集运行记录，存在一天空缺；因 Trending/热榜为当日口径、隔日不可复现，本期只补 09-18，不回填 09-17**
- 新建：[[每日AI素材-2026-09-18-摘要]]（摘要页：四大信号——agent 工具/记忆可靠性集群、自维护 Wiki 开源再印证、知识库防腐坏、可控 RSI（书生·星河）；含待核实 6 项）
- 更新：[[Agent]]（frontmatter +1 source；新增「每日素材信号（2026-09-18）」H3 子节，规避已有 09-16 趋势节 H2；相关链接 +1；updated→2026-09-18，status 维持 growing）
- 更新：[[Harness自进化]]（「与相关概念的边界」节补「外部印证（2026-09-18）」出链，WeKnora/Agents-Flex 再印证自维护 Wiki + ECC/PRISM/MERIT/ResidualAuth 量化「memory/tools 不可靠」失败面；相关链接 +3；updated→2026-09-18，status 维持 seed）
- 台账：index.md（37→38 页、素材 20→21）、log.md、growth.md、lifecycle.md
- Propose 清单（本次遵守）：新建摘要页 1 + 增量归并 Agent.md + Harness自进化.md（外部印证）；§8 当前无活跃 disposition（默认 no-goal 自由生长），不强行造内容；已识别冲突点=Agent.md 已有 09-14/15/16 趋势节与 09-16 信号 H3 → 新增 09-18 信号沿用 H3 子节规避重复 H2
- 待核实：GitHub stars today 为页面声称值未独立核验（ECC 261K 疑似含 fork/镜像计数存疑）；Gitee 列表为存量热度非当日趋势；arXiv 2609.xxxxx ID 未独立核验；PRISM/ExecCritic/MERIT/ResidualAuth/SchemeArena 来自 cnblogs 媒体日报未对照原文；书生·星河数值来自新浪财经未对照论文；WeKnora/Agents-Flex 与本项目「自生长」属类比非同一来源
- 备注：未改 `.obsidian/`、未构建、未提交（入库后由 step 6 自动 commit + push）

## [2026-09-16] ingest | 每日AI素材-2026-09-16（四渠道头部采集 + 增量入库）

- 来源：`raw/00-Inbox/每日AI素材-2026-09-16.md`（本自动化第 0 步采集产出；GitHub Trending / Gitee LLM / CSDN 热榜 / arXiv cs.AI + WebSearch 热点补充；HuggingFace papers 本环境 fetch failed → 改用 WebSearch 补 5 条；raw 既有文件零改动）
- 差异检测：raw/ 知识素材共 20 篇（排除 `80-Templates` 与 `articles/README.md`）；19 篇已被引用，本次新建每日素材文件 = 唯一新增未消化素材（经 Grep 核对 `每日AI素材-2026-09-14/15` 均已引用、`2026-09-16` 尚未引用，确认待消化集合 = 1）
- 新建：[[每日AI素材-2026-09-16-摘要]]（摘要页：四渠道亮点提炼 + 自维护 Wiki 开源印证 + 知识库防腐坏同构 + Plan Injection 安全议题；含待核实 5 项）
- 更新：[[Agent]]（frontmatter +1 source；新增「每日素材信号（2026-09-16）」H3 子节，避开已存在的 09-16 Proteus 趋势节；相关链接 +1；updated 维持 2026-09-16，status 维持 growing）
- 更新：[[Harness自进化]]（「与相关概念的边界」节补「外部印证（2026-09-16）」出链，WeKnora/Social Harness/ECC 印证「演化对象=harness」；status 维持 seed，sources 不变）
- 台账：index.md（36→37 页，素材 19→20）、log.md、growth.md、lifecycle.md
- Propose 清单（本次遵守）：新建摘要页 1 + 增量归并 Agent.md + 网络强化 Harness自进化.md；§8 当前无活跃 disposition（默认 no-goal 自由生长），不强行造内容；已识别冲突点=Agent.md 已有 09-16 趋势节（Proteus 来源）→ 改用 H3 子节规避重复 H2
- 待核实：GitHub stars today 为页面声称值未独立核验；Gitee 列表为存量热度非当日趋势；arXiv 2609.xxxxx ID 未独立核验；Plan Injection/Gavel/Social Harness/BPO 来自媒体日报未对照原文；WeKnora 与本项目「自生长」属类比非同一来源
- 备注：未改 `.obsidian/`、未构建、未提交（入库后由 step 6 自动 commit + push）

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