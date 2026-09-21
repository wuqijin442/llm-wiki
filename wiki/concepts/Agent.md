---
title: Agent
aliases:
  - AI智能体
  - 智能体
tags:
  - AI
  - 概念
  - 框架
category: concepts
created: 2026-09-12
updated: 2026-09-21
sources:
  - "[[raw/20-Tech/RAG检索增强生成]]"
  - "[[raw/20-Tech/为什么转岗AI大模型应用]]"
  - "[[raw/00-Inbox/每日AI素材-2026-09-14]]"
  - "[[raw/00-Inbox/每日AI素材-2026-09-15]]"
  - "[[raw/00-Inbox/每日AI素材-2026-09-16]]"
  - "[[raw/00-Inbox/每日AI素材-2026-09-18]]"
  - "[[raw/00-Inbox/每日AI素材-2026-09-19]]"
  - "[[raw/00-Inbox/每日AI素材-2026-09-19-晚]]"
  - "[[raw/00-Inbox/每日AI素材-2026-09-20]]"
  - "[[raw/00-Inbox/每日AI素材-2026-09-20-晚]]"
  - "[[raw/00-Inbox/每日AI素材-2026-09-21]]"
  - "[[raw/articles/2026-09-16-Proteus-自进化harness框架-仓库精读]]"
description: AI 智能体：能调工具、能续跑的自主任务执行体，RAG 之上记忆层是其关键组件。
status: growing
---

# Agent

## 定义

AI 智能体（Agent）：能调用工具、持续执行任务的大模型应用形态，配合 harness 完成系统设计。

## 核心思想

Agent 不仅回答，还能行动：模型决策 + 工具调用 + 状态记忆。记忆层（尤其 [[RAG]] 之上叠加的五层记忆栈）决定 Agent 能否多轮长程执行。

## 应用场景

- 调工具、能续跑的自主 Agent
- 设备运维工单摘要与异常归因 Agent
- 多 Agent 协作系统

## 关键子概念

- **记忆层**：Working / Short-term / Long-term / Semantic / Episodic Memory
- **记忆晋升门控**：session→user→tenant→global 分级，防 guardrail drift 与记忆投毒（对标 OWASP ASI06）
- **Harness**：包裹模型的外部系统设计（2026 新护城河）

## 参考实例

- `ai-llm-app-roadmap/docs/05-Agent智能体与Harness`
- RAG 记忆层五层栈（2026 新增）

## 趋势观察（2026-09-14）

GitHub Trending 当日多条 agent 基础设施项目集中冲榜，印证"Harness 是 2026 新护城河"的判断，且竞争焦点从单 agent 能力转向**底座/工具层**：

- 联网能力：Agent-Reach（给 agent 联网"眼睛"，零 API 费 CLI）
- 技能治理：agent-skills（面向编码智能体的安全、已校验 skill 注册表）
- 垂直框架：TradingAgents（多智能体金融交易）、Claude-Red（agent 安全技能库）

> 来源与细节见 [[每日AI素材-2026-09-14-摘要]]（信号提炼）。skill 注册表的"安全校验"本质是一种 agent 分权治理，关联 [[AI智能体分权治理]]。

## 趋势观察（2026-09-15）

GitHub Trending 当日头部再次出现多条 agent 基础设施集中冲榜，且**递归自我改进（RSI）/ agent 自我演化**成为 arXiv + 热点补充中最密集的议题，从「单 agent 能力」与「底座工具层」进一步推进到「agent 如何自我变强」：

- agent 工具化延续冲榜：`alibaba/open-code-review`（代码审查）、`alphaXiv/OpenResearch`（研究 agent）、`pacifio/atlas`（agent 源码管理）、`addyosmani/agent-skills`（94.5K Star 的 skill 合集）
- RSI 热点密集：`RSIAgent`（arXiv 2609.15364，据媒体报道不更新参数即超 GPT-6 Astra）、`Generalized Agent Iteration`（统一 RSI 形式化框架）、`Atria Dawn`（agentic 超级智能论述）
- agent 工具接口与 eval 盲点被量化：`Is Bash All You Need?`（纯 bash 比 typed tools 高 21.8–24.5 分）、`Mechanics of a Swarm`（eval 环境未记录读/结果致协调失效）

> 来源与细节见 [[每日AI素材-2026-09-15-摘要]]（信号提炼）。RSI / 自改进闭环 / eval 可观测性关联 [[AI智能体工程方法论]]；Agentic Visual RAG 关联 [[RAG]]。

## 趋势观察（2026-09-16）

09-15 的 RSI 热点从「议题」落到「可复现实验仪器」：精读 [[Proteus]]（MIT，研究预览）可见 harness 自进化的工程化路径——**演化对象从权重移到 harness**（prompts/memory/skills/tools/控制循环），用 `observe → propose → act → reflect` 四相位 episode 让 agent 改写自己，并用结构距离 / 行为距离 / 结晶测试测量变化，而非只报 benchmark 分。与 09-14「Harness 是 2026 护城河」判断衔接：护城河不只是「有 harness」，还包括**能否测量 harness 怎么长**。

> 概念展开见 [[Harness自进化]]、[[自进化测量标尺]]；与本库自生长机制的对照见 [[双环知识飞轮]]。

### 每日素材信号（2026-09-16）

同日的四渠道头部采集（见 [[每日AI素材-2026-09-16-摘要]]）把 09-15 的「harness 护城河 + RSI」判断进一步坐实到**生产/社区级证据**：

- **自维护 Wiki 开源实现**：`Tencent/WeKnora`（Go）把原始文档变成可查询 RAG + 自主推理 agent + **自维护 Wiki**，与本库「LLM Wiki 自生长」高度同构——外部印证「知识库能自己长」已是可落地产品形态，不止方法论设想。
- **agent harness 工具持续冲榜**：`alibaba/open-code-review`（代码审查）、`cloudflare/security-audit-skill`（安全审计 skill）、`addyosmani/agent-skills`（skill 合集）、`affaan-m/ECC`（agent harness 性能优化系统）延续「竞争焦点在底座/工具层」的走势。
- **安全边界被量化**：热点补充的 `Plan Injection` 攻击可在输入上下文植入良性推理逻辑、绕过 CoT 监视器诱导有害行为——关联 [[意图路由与服务端兜底]] 的「LLM 失败时服务端确定性兜底」必要性。
- **多 agent 节制设计**：`Decomposition Buys Integrity, Not Yield` 指出任务切分提升完整性但不提升、甚至降低整体产出，对「无脑加 agent」orthodoxy 形成反驳证据。

> 单 agent 能力 → 底座/工具层 → 自我演化 → 生产印证，是本库 [[Agent]] 趋势观察（09-14 → 09-15 → 09-16）的主线。

### 每日素材信号（2026-09-18）

同日的四渠道头部采集（见 [[每日AI素材-2026-09-18-摘要]]）把主线从「harness 能不能演化」推进到「**演化出来的 harness 是否可靠、记忆撤销是否彻底**」——这是 09-16「生产印证」之后的必然追问：

- **工具/记忆可靠性成为最密集议题**：GitHub 工具层延续冲榜（`cloudflare/security-audit-skill`、`alibaba/open-code-review`、`addyosmani/agent-skills`、`affaan-m/ECC`）；CSDN 从实战侧补「AI Agent 越界之后：为什么只有沙箱还不够」「Agent 工具设计原则」；WebSearch 热点补充集中出现 `PRISM` / `ExecCritic` / `MERIT` / `Agents Trust Tools Too Much` / `ResidualAuth` / `SchemeArena` 等——指向同一结论：agent 不可靠的根因常在**工具回传不可信、记忆未真正撤销、权限边界未硬校验**，而非模型本身。
- **记忆撤销失效被量化**：`ResidualAuth` / `Revoked but Still Authoritative` 显示写入已撤销政策后，5 种记忆系统无一种默认完整执行撤销；`MERIT` 显示向量检索遇更新事实波动大、近半正确信息未被执行——直接坐实 [[Harness自进化]] 中"memory 是被演化、也需要被约束的 surface"。
- **自维护 Wiki 开源再印证**：`Tencent/WeKnora`（自维护 Wiki）、`Agents-Flex`（Java 框架显式支持 LLM Wiki）延续 09-16 的"自生长知识库可落地"证据链。
- **知识库会腐坏**：CSDN「知识库防腐坏」把衰减讲成需要巡检/软下架/灰度回滚的运维问题，关联 [[LLM-Wiki-vs-RAG]]。

> 主线续接：单 agent 能力 → 底座/工具层 → 自我演化 → 生产印证 → **可靠性/防腐坏（09-18）**。

### 每日素材信号（2026-09-19）

同日的四渠道头部采集（见 [[每日AI素材-2026-09-19-摘要]]）把「harness 是护城河」推进到「**harness 是可拆解测量的科学对象**」：

- **harness 论文密集群**：arXiv 当日头部同时出现 `Harness Design for Coding Agents`（组件级实证：固定执行循环，单独变换 planning/action space/context management）、`SoL-Pi`（harness 层递归 auto-research loops）、`How Do Agent Harnesses Create Value?`——护城河被**拆开量化**，从口号变成可复现实验。
- **skill 生态走向安全审计化**：当日 Trending 头名 `cloudflare/security-audit-skill`（+3,019 today）是给 agent 产出做验证的 skill；`SkillAA` 给 skill-graph 更新加定向验证与回滚——skill 治理开始具备事务契约特征，关联 [[AI智能体分权治理]]。
- **自维护 Wiki 第三次外部印证**：LangChain `OpenWiki`（Credit Genie 知识保鲜 + Self-Correcting Memory）续接 WeKnora（09-16）→ Agents-Flex（09-18）证据链，「知识库能自己长且需自修正」获得三源独立印证。
- **agent 工作负载反向塑造底座**：`DeepSeek-V4.1-Flash` decode 16B / prefill 8B 不对称激活、`Atria Dawn Preview` 直接以 agentic 命名旗舰——底座架构取舍由 agent 场景驱动。

> 主线续接：… → 可靠性/防腐坏（09-18）→ **组件级科学化（09-19）**。

### 每日素材信号（2026-09-19-晚）

同日晚间批次（见 [[每日AI素材-2026-09-19-晚-摘要]]）把「harness 组件级科学化」主线推进到「**harness 成本被单独度量**」，并补上两条早间未覆盖的新信号（边缘自动化模型 / 多模态推理优化）：

- **harness 成本实证落地**：`HarnessTax`（UC Berkeley + Arena，09-16）显示同一模型在 Claude Code / Codex CLI / Pi 三套 harness 上 SWE-bench Lite + Terminal-Bench 2.0 **成功率几乎不变、成本差最高 5x**，根因是 harness 每请求输入 token 量（~27k / ~15k / ~2.6k）。「harness tax」是 09-19 早间「harness 可拆解测量」主张的**成本侧补完**——不只测「准不准」，还要测「每调用花多少」。
- **coding-agent / skills 生态延续霸榜**：GitHub 当日头名 `cloudflare/security-audit-skill`（+3,162 today）、`addyosmani/agent-skills`（+547）、`anthropics/claude-code 2.1.277 支持 AGENTS.md 回退`、`MiniMax Code CLI` 开源（MIT）——延续「竞争焦点在底座/工具层」主线，并出现「AGENTS.md 作为跨工具项目指令规范」的生产侧同构（关联本库 `AGENTS.md`）。
- **微型/边缘端自动化基础模型（新信号）**：`cactus-compute/needle`（2-bit、8–29MB，跑 MCU/手机）、`Cactus Needle 3`（Raspberry Pi 5 上 4k tok/s decode）、`AutoArk Edge0`（35B 跑 SSD Mac mini，3GB 活跃内存 20.4 tok/s）——自动化模型开始下沉到消费级/嵌入式硬件。
- **多模态/视频推理优化（新信号）**：`vLLM + 英伟达 PyNvVideoCodec` 硬件视频解码、`Kijai MiniMax-H3 VAE` 在 RTX 3060 提吞吐、`LingBot-World 2.0 1.3B` 单卡 6→16 FPS。
- **AI 治理/合规升温（新信号）**：加州 `AI kill switch` 行政令、弗州数据中心放缓 + AI 任务组、三巨头自建自监管体、苹果参考图像防伪——治理从「模型行为」前移到「电力/基础设施/审计」，关联 [[iOS内部分发与App Store合规]] 的「合规即工程约束」。

> 主线续接：… → 组件级科学化（09-19）→ **成本侧度量 + 边缘/多模态/治理外溢（09-19 晚）**。

### 每日素材信号（2026-09-20）

同日的四渠道头部采集（见 [[每日AI素材-2026-09-20-摘要]]）把「harness 组件级科学化」主线推进到「**世界模型与 harness 设计同周密集实证**」，并延续 coding-agent/skills 生态霸榜：

- **harness 设计实证再补强**：[arXiv:2609.20804《An Empirical Study of Harness Design for Coding Agents》](https://arxiv.org/abs/2609.20804) 用固定执行循环、单独变换 planning/action space/context management 的组件级实验，把「harness 护城河」落成可复现实验；[NVIDIA SoL-Pi](https://agihunt.info/en/daily/2026-09-20?f=dr) 在 harness 层跑自研究循环（Action Fusion / Online Context Compact / ObservationPack），宣称约 50% 更少 token、约 33% 更低 API 成本；[C2C（Cache-to-Cache，清华 + 无问芯穹，ICLR 2026）](https://agihunt.info/en/daily/2026-09-20?f=dr) 从多 agent 通信中去掉文本、用 Neural Fuser 嫁接 KV-cache。
- **coding-agent / skills 生态连续霸榜**：GitHub 当日头名 [cloudflare/security-audit-skill](https://github.com/cloudflare/security-audit-skill)（+3,162 today）、[addyosmani/agent-skills](https://github.com/addyosmani/agent-skills)（+547）、[anthropics/claude-code](https://github.com/anthropics/claude-code)、[cactus-compute/needle](https://github.com/cactus-compute/needle)（+207，2-bit 微型自动化模型）——延续「竞争焦点在底座/工具层」主线。
- **世界模型 / 通用表征新信号**：[JEPA-Anything（arXiv:2609.20800）](https://agihunt.info/en/daily/2026-09-20?f=dr) 基于 OPF 的域无关世界模型，跨 7 领域把单 JEPA 潜目标拆成可检视互补因子。
- **概率即输出（JEV 类）新信号**：[TypeSafe Jev](https://agihunt.info/en/daily/2026-09-20?f=dr) 不生成文本只输出概率；[Von（395M）](https://agihunt.info/en/daily/2026-09-20?f=dr) 标榜 CPU 平替——「少 token、低延迟、概率化」成为一类新形态。
- **医疗影像开源（国内普惠路线）**：[DAMORADAR（达摩院 + 浙一，刊《Science》）](https://www.toutiao.com/article/7687226704070279689/) 单模型覆盖 18 解剖结构、识别 146+ 病症并开源。

> 主线续接：… → 成本侧度量 + 边缘/多模态/治理外溢（09-19 晚）→ **世界模型 + harness 设计密集实证 + 概率即输出（09-20）**。

### 每日素材信号（2026-09-20 晚）

同日晚间批次（见 [[每日AI素材-2026-09-20-晚-摘要]]）把「coding-agent / harness 安全审计霸榜」主线同日强化，并补上两条早间未覆盖的新信号（computer-use / Generative UI 新形态、开源治理与 Agent 安全护栏的中文社区升温）：

- **coding-agent / harness / 安全审计 skill 同屏霸榜（同日强化）**：GitHub 当晚同屏出现 [affaan-m/ECC](https://github.com/affaan-m/ECC)（今日 +1,012）、[cloudflare/security-audit-skill](https://github.com/cloudflare/security-audit-skill)（今日 +3,155）、[addyosmani/agent-skills](https://github.com/addyosmani/agent-skills)（+556）、[anthropics/claude-code](https://github.com/anthropics/claude-code)（+483）、[coder/coder](https://github.com/coder/coder)（+402）——延续「竞争焦点在底座/工具层」主线，且以安全/审计为卖点，关联 [[AI智能体分权治理]]。
- **computer-use 2.0 与 Generative UI 新形态**：[trycua/cua](https://github.com/trycua/cua)（Scale computer-use 2.0，今日 +859）把 agent 的 action surface 从写代码外溢到「操控桌面/跨系统执行」；[vercel-labs/json-render](https://github.com/vercel-labs/json-render)（The Generative UI framework，今日 +585）代表「生成式界面」范式。
- **Gitee Agents-Flex 把 LLM Wiki 列为内置能力**：[Agents-Flex/Agents-Flex](https://gitee.com/agents-flex/agents-flex)（JAVA 框架、Gitee 2.9K★、3天前更新）功能清单显式包含「LLM Wiki」——「自生长知识库」成为社区框架级能力的**第三次独立印证**（WeKnora 09-16 → Agents-Flex 09-18 → 本次 Gitee 热度再确认），关联 [[自生长知识库实战-苍何]]、[[双环知识飞轮]]。
- **CSDN 接口恢复 + 中文社区热点**：[22秒攻击窗口下的防御重构：Agent 安全护栏实践](https://blog.csdn.net/CC1991_/article/details/165754156)（热度 9346）与[开源 vs 开权重控制权之争](https://blog.csdn.net/weixin_74809706/article/details/164885385)（热度 14953）反映 Agent 安全与开源治理升温，关联 [[AI智能体工程方法论]]「合规即工程约束」。
- **WebSearch 补充推进成本度量主线**：[Inference routing economics（arXiv 2609.15992）](https://arxiv.org/abs/2609.15992) 把「harness tax」从单 harness 对比推进到**推理路由层**（阈值调度廉价/贵模型降本）；[REALM（2609.16055）](https://arxiv.org/abs/2609.16055) 把记忆当作持续演化的生命周期，强化本库 [[Harness自进化]] 的 memory surface；[Interaction-Induced Knowledge Narrowing](https://link.springer.com/article/10.1007/s44163-026-02101-6) 关联 [[RAG]]。

> 主线续接：… → 世界模型 + harness 设计密集实证 + 概率即输出（09-20）→ **computer-use/Generative UI 新形态 + 推理路由/记忆再巩固补强成本度量（09-20 晚）**。

### 每日素材信号（2026-09-21）

同日的四渠道头部采集（见 [[每日AI素材-2026-09-21-摘要]]）把「harness 是护城河」主线推进到「**技能从代码库挖掘成行业共识 + harness 建议随上下文窗口失效 + 技能成为协议级资源 + coding agent 外泄成具体威胁**」：

- **agent 技能从代码库挖掘成共识（三团队独立）**：[CodeMidas（arXiv:2609.22068）](https://arxiv.org/abs/2609.22068) 从开源代码直接合成 23 语言 5,545 个可执行 coding RL 环境（GRPO 训 MiMo-V2.5 使 DeepSWE +11.7%）；[GraphSkillEvo（arXiv:2609.21749）](https://arxiv.org/abs/2609.21749) 用图结构技能 + 进化优化超 SkillOpt；智源 [DisCo/AREX-Skill](https://ima.qq.com/wiki/?shareId=46f5bcd8d0e86361201f2fe35852c35ddf673179f11f1ab332077d95ee9ec7e7) 把 1000 仓库蒸馏成 5000+ 技能（基准最高 +134.3%）。结论：agent 训练稀缺资源是「经验证的环境+技能」，代码库最便宜来源——直接呼应 [[Harness自进化]]「skill 是被演化、也需约束的 surface」。
- **harness 建议随上下文窗口增长失效（176 设定消融）**：上下文管理在 SWE-Bench 增益从 32k 窗口 35.7 分跌到 128k 窗口 2.7 分——harness 决策须与模型升级绑定再测量节奏，而非一次性设计文档（强化 [[自进化测量标尺]] 须绑定上下文窗口变量）。
- **Skills over MCP 定稿（SEP-2640 Final）**：agentskills.io 格式以 `skill://` URI 经 MCP 作资源提供（skills/list、skills/get），技能从本地目录变远程可下发，带来供应链后果——与「AGENTS.md 作为跨工具指令规范」同构。
- **coding agent 数据外泄成具体威胁模型（延续安全审计化）**：智谱 ZCode 每次 prompt 打包完整工作区（含全量 .git 历史）加密上传阿里云 OSS、无 UI 开关；Patronus Scanner 上线 MCP 注入扫描；NIST Plugin4Shell 同周出现——关联 [[意图路由与服务端兜底]]「出口需显式核算」。

> 主线续接：… → computer-use/Generative UI 新形态 + 推理路由/记忆再巩固（09-20 晚）→ **技能从代码库挖掘成共识 + harness 建议随上下文窗口失效 + 技能协议化 + 外泄威胁模型（09-21）**。

## 相关链接

- [[RAG]] - Agent 记忆层的基础
- [[AI应用开发]] - 应用层核心组件之一
- [[每日AI素材-2026-09-14-摘要]] - 2026-09-14 agent 工具化冲榜趋势信号
- [[每日AI素材-2026-09-15-摘要]] - 2026-09-15 agent 工具化 + RSI 热点趋势信号
- [[每日AI素材-2026-09-16-摘要]] - 2026-09-16 自维护 Wiki 开源印证 + agent harness 工具 + Plan Injection 安全议题
- [[每日AI素材-2026-09-18-摘要]] - 2026-09-18 agent 工具/记忆可靠性 + 自维护 Wiki 再印证 + 知识库防腐坏 + 可控 RSI
- [[每日AI素材-2026-09-19-摘要]] - 2026-09-19 harness 组件级科学化 + skill 安全审计化 + OpenWiki 第三次印证
- [[每日AI素材-2026-09-19-晚-摘要]] - 2026-09-19 晚 harness 成本度量(HarnessTax) + coding-agent/skills 生态 + 边缘自动化模型 + 多模态推理优化 + AI 治理合规
- [[每日AI素材-2026-09-20-摘要]] - 2026-09-20 harness 设计实证(2609.20804) + 世界模型(JEPA-Anything) + 概率即输出(Jev/Von) + 医疗影像开源(DAMORADAR)
- [[每日AI素材-2026-09-20-晚-摘要]] - 2026-09-20 晚 coding-agent/harness 安全审计霸榜延续 + computer-use/Generative UI 新形态 + Agents-Flex 把 LLM Wiki 列为内置 + CSDN 恢复且 Agent 安全护栏成中文热点 + 推理路由经济学/记忆再巩固补强 harness 成本度量
- [[每日AI素材-2026-09-21-摘要]] - 2026-09-21 agent 技能从代码库挖掘成共识 + harness 建议随上下文窗口失效 + Skills over MCP 定稿 + coding agent 外泄威胁模型 + Paper2Agent 论文即可执行 agent
- [[AI智能体分权治理]] - agent-skills 注册表的安全校验本质
- [[AI智能体工程方法论]] - RSI / 自改进闭环 / agent 工具接口 / eval 盲点
- [[Proteus]] - harness 自进化实验框架（2026-09-16 精读）
- [[Harness自进化]] - 演化对象从权重到 harness
- [[自进化测量标尺]] - 测「进化了没有」的三把尺
- [[GitHub-AI周报2026-08-09]] - 2026-08-09 GitHub 全赛道周报（技能层占增量 50% / 本地推理双雄趋势）