---
title: Harness自进化
aliases:
  - harness self-evolution
  - Agent harness 自改进
  - 权重之外的自进化
tags:
  - AI
  - 智能体
  - 概念
  - 方法论
category: concepts
created: 2026-09-16
updated: 2026-09-22
sources:
  - "[[raw/articles/2026-09-16-Proteus-自进化harness框架-仓库精读]]"
  - "[[raw/00-Inbox/每日AI素材-2026-09-19-晚]]"
  - "[[raw/00-Inbox/每日AI素材-2026-09-20]]"
  - "[[raw/00-Inbox/每日AI素材-2026-09-20-晚]]"
  - "[[raw/00-Inbox/每日AI素材-2026-09-21]]"
  - "[[raw/00-Inbox/每日AI素材-2026-09-22]]"
  - "[[raw/00-Inbox/refresh-verify-2026-09-22]]"
description: 自改进的重心从模型权重移到 harness（prompts/memory/skills/tools/控制循环）；用四相位 episode 与事务契约让 harness 安全地改写自己。
status: growing
confidence: 高
scope: 以 Proteus v0.3.0 设计为蓝本的实验框架视角；非训练时论文综述
---

# Harness自进化

## 定义

**Harness 自进化**：不更新模型权重，而让 agent 在多轮**上下文新鲜**的 episode 中改写自己运行其上的 harness——提示词、记忆、技能、工具与控制循环——并由外部框架负责快照、选择、回滚与测量。

Proteus 的开篇判断：agent 自改进的重心正从 **权重** 移向 **harness**。演化的是「模型怎么被使用」，不是「模型是什么」。

## 核心思想

### 1. 演化对象是一组可声明的 Surface

不是笼统的「让 agent 变聪明」，而是把 harness 拆成**可编辑、可持久、可分别计量**的面（instructions、notes/memory、tools、skills、自身源码…）。Surface 以**数据**声明，测量层不用硬编码名字——换 harness 不用改尺子。

### 2. Episode：四相位 + 上下文新鲜

```
observe  →  propose  →  act  →  reflect
盘点       选一项改进   改自己    验证与定下一步
```

- 每个相位都是**新鲜上下文**；跨 episode 存活的只有 harness 文件（+ 可选的有界操作交接，且交接住在被测快照之外，不算演化记忆）。
- 有目标：四相位都注入 goal 文本；上轮 OBSERVE 可见分只进 observe。
- 无目标：中性探索语言；reflect 记「效果与意外」，不称之为改进；agent 可自立临时目标作为演化状态。

### 3. 事务契约：框架拥有事务，adapter 拥有执行

| 归框架 | 归 adapter |
| --- | --- |
| 相位提示组装、快照、晋升/回滚、评估路由、记录、resume | 相位如何真的跑起来、trace 怎么解析、disposition 装在哪、candidate 怎么校验 |

Staged activation（可选能力）：本 episode 四相位共读**同一冻结 active 快照**；可写 candidate 的编辑要到下 episode 才激活。失败（构建/启动）过不了无模型的边界可行性门 → 回滚，失败树留给下轮当 repair base。**坏掉的演化提案不得控制下一轮运行时。**

### 4. 认识论协议：评估器是证据，不是成功定义

外部 evaluator / benchmark 分数是对演化的**证据**，不自动等于「目标已完成」。窄基准可能完全操作化匹配的目标；宽目标需要 harness 自建额外测试。不为了仪式而堆评估器。无外部目标时不假设目标。

### 5. 归因要求单一可移除扰动

想回答「某个 action preference 有没有塑造 harness」，t=0 只植入**一个**可移除的 Disposition（见 `[[自进化测量标尺]]`）。否则后续分歧无法归因。

## 何时用、何时不用

| 适合 | 不适合 |
| --- | --- |
| 研究：无目标演化会长出什么 / 初始条件是否留下痕迹 | 直接当生产 agent 服务器 |
| 比较不同 harness 面、不同扰动、不同目标条件 | 没有沙箱就让 agent 改自己能执行的代码 |
| 需要可复现的 evolution history（git per episode） | 只想要一个更高 benchmark 分数的黑盒 |

## 与相关概念的边界

- **训练 / 微调**：改权重；Harness 自进化改的是使用权重的脚手架。
- **自愈闭环**（`[[自愈闭环设计]]`）：生产故障下的改码重启通道，目标是可用性；Harness 自进化是实验演化，目标是可测量的形态变化。两者都涉及「agent 改代码」，信任模型与验收标准不同。
- **LLM Wiki 自生长**（`[[自生长知识库实战-苍何]]`、`[[双环知识飞轮]]`）：演化对象是**知识库编译产物**，主体是维护库的 agent+人；Harness 自进化的演化对象是**agent 自己的运行脚手架**。都关心「留下了什么结构化变化」，但被测物不同。
- **RSI（递归自我改进）**：见 `[[每日AI素材-2026-09-15-摘要]]`；Proteus 提供的是其中 harness 路径的实验仪器，不是完整 RSI 理论。
- **外部印证（2026-09-16）**：`[[每日AI素材-2026-09-16-摘要]]` 中 `Tencent/WeKnora`（自维护 Wiki）、`Agentic Societies Need a Social Harness`、`affaan-m/ECC`（agent harness 优化系统）从生产/社区侧印证「演化对象=harness」的判断——自维护知识库、社会协调 harness、harness 性能优化都已是可落地形态。
- **外部印证（2026-09-18）**：`[[每日AI素材-2026-09-18-摘要]]` 从两个方向补强——① `Tencent/WeKnora`（自维护 Wiki）与 `Agents-Flex`（Java 框架**显式支持 LLM Wiki**）再次印证"自生长知识库"已是社区框架能力，不止个人方法论；② `affaan-m/ECC` 把 harness 优化系统化为 skills/instincts/memory/security 的成品，且当日热点补充（PRISM/ExecCritic/MERIT/ResidualAuth/SchemeArena）量化了"memory/tools 不可靠"这一 harness 必须被测量和约束的失败面。
- **外部印证（2026-09-19-晚）**：`[[每日AI素材-2026-09-19-晚-摘要]]` 把「演化对象=harness」从「组件级科学化」推进到「**成本侧度量**」——`HarnessTax`（UC Berkeley + Arena）以 SWE-bench Lite + Terminal-Bench 2.0 实证：同一模型在三套 harness（Claude Code / Codex CLI / Pi）上**成功率几乎不变、成本差最高 5x**，根因是 harness 每请求输入 token 量（~27k / ~15k / ~2.6k）；同日 arXiv 2609.20474《How Do Agent Harnesses Create Value?》从规划信息（planning information）与释放控制（release control）角度量化 harness 价值来源。这与本概念「harness 可测量、需被测量（不只测模型）」主张同构，也为 `[[自进化测量标尺]]` 提供真实度量动机：先量清 harness 的 token 开销与价值贡献，再谈演化。
- **外部印证（2026-09-20）**：`[[每日AI素材-2026-09-20-摘要]]` 把「harness 可测量」主线推进到**设计层实证密集化**——① [arXiv:2609.20804《An Empirical Study of Harness Design for Coding Agents》](https://arxiv.org/abs/2609.20804) 用固定执行循环、单独变换 planning/action space/context management 的组件级实验，把「harness 护城河」落成可复现实验；② [NVIDIA SoL-Pi](https://agihunt.info/en/daily/2026-09-20?f=dr) 在 harness 层跑自研究循环（Action Fusion / Online Context Compact / ObservationPack），宣称约 50% 更少 token、约 33% 更低 API 成本；③ [C2C（Cache-to-Cache，清华 + 无问芯穹，ICLR 2026）](https://agihunt.info/en/daily/2026-09-20?f=dr) 从多 agent 通信中去掉文本、用 Neural Fuser 嫁接 KV-cache。三者共同坐实本概念「harness 是组件级可测量、需被测量的科学对象」主张，并为 `[[自进化测量标尺]]` 提供真实度量场景：token 开销、机制存活率、跨层 cache 嫁接效率均可成为 harness 演化的被测 surface。
- **外部印证（2026-09-20 晚）**：`[[每日AI素材-2026-09-20-晚-摘要]]` 把「harness 可测量、需被测量」主张从「设计层实证」推进到**成本侧度量的两个新切面**——① [Inference routing economics（arXiv 2609.15992）](https://arxiv.org/abs/2609.15992) 用「廉价模型信心不足才调用贵模型」的阈值为推理网络动态路由不同成本/能力 LLM，开源模型实验大幅降本且维持性能；这与 09-19 晚 HarnessTax「同一模型三套 harness 成本差最高 5x」同构——**路由即一种 harness 成本优化面**，给 [[自进化测量标尺]] 提供「每请求 token / 路由决策」被测 surface。② [REALM（arXiv 2609.16055）](https://arxiv.org/abs/2609.16055) 把 agent 记忆当作持续演化的生命周期（异构认知图 + 检索反馈再巩固），而非静态向量库——再次坐实本概念「memory 是被演化、也需被约束的 surface」（与 09-18 ResidualAuth/MERIT 量化「记忆撤销失效」构成正反两面）。③ 同日 [Agents-Flex/Agents-Flex](https://gitee.com/agents-flex/agents-flex)（Gitee 2.9K★、3天前更新）把「LLM Wiki」列为框架内置能力，使 [[双环知识飞轮]] / [[自生长知识库实战-苍何]] 的「知识库自生长」主张再获社区框架级印证。
- **外部印证（2026-09-21）**：`[[每日AI素材-2026-09-21-摘要]]` 把「演化对象=harness、且 harness 可测量需被测量」主张推进到**技能作为可进化 surface 的具体来源 + 测量必须绑定上下文窗口**——① 同日 [CodeMidas（arXiv:2609.22068）](https://arxiv.org/abs/2609.22068) / [GraphSkillEvo（arXiv:2609.21749）](https://arxiv.org/abs/2609.21749) / 智源 [DisCo/AREX-Skill](https://ima.qq.com/wiki/?shareId=46f5bcd8d0e86361201f2fe35852c35ddf673179f11f1ab332077d95ee9ec7e7) 三个独立团队同时得出「从现有代码库提炼 agent 技能/环境」是 agent 训练的关键路径（5,545 任务 / 图结构技能进化 / 5000+ 蒸馏技能，基准最高 +134.3%）——**skill 库本身就是 harness 演化的一类被测 surface**，与 09-20 C2C「多 agent 通信嫁接 KV-cache」同构：技能/记忆/通信都是可被演化、也需被约束的面。② [Daily AI Brief 2026-09-21](https://wire.rundatarun.io/briefs/2026-09-21) 报道的 176 设定消融显示 harness 建议随上下文窗口增长失效（SWE-Bench 增益 32k→128k 从 35.7 跌到 2.7 分）——直接给 [[自进化测量标尺]] 一个硬约束：**结构/行为距离测量必须标注被测模型的上下文预算**，否则「harness 变好」的读数会因窗口增大而失真。③ Skills over MCP（SEP-2640 Final，`skill://` URI，2026-09-22 经 modelcontextprotocol.io 官网核实）使技能从本地目录升级为协议级可寻址、可远程下发的资源，供应链后果使 skill surface 的治理从「项目内约定」外溢到「协议层」；**但「Final」仅协议层定稿，官方 Go / Python / TypeScript / C# SDK 的 Skills 支持 PR 仍 open，客户端尚不能端到端消费——规范稳定 ≠ 客户端可用**（详见 [[refresh-verify-2026-09-22]]）。
- **外部印证（2026-09-22）**：`[[每日AI素材-2026-09-22-摘要]]` 把「演化对象=harness、且 harness 可测量需被测量」主张推进到「**harness 被蒸馏、且被厂商商品化为运行时**」两个新切面——① [arXiv:2609.24974《Harness-Zero: Harness Distillation via Agent-as-Harness》](https://arxiv.org/abs/2609.24974) 把 harness distillation 作为一个新课题提出（Agent-as-Harness），本库「演化对象=harness」从口号落成可研究问题；② 当日 GitHub Trending 头部队几乎全是 agent/harness 运行时基础设施（[agent-substrate/substrate](https://github.com/agent-substrate/substrate) +498 today、[google/ax](https://github.com/google/ax) +2,324 today 的 agentic 编排运行时、[dream-num/univer](https://github.com/dream-num/univer) 的 "Office Harness for AI Agents"、[superdesigndev/treg](https://github.com/superdesigndev/treg) 的 "OpenRouter for agent tools"、[browser-use/video-use](https://github.com/browser-use/video-use) 的 coding-agent 视频编辑），与 09-19「组件级科学化」、09-19 晚「成本侧度量」、09-20「设计层实证」构成连续证据链：harness 正从被测量的对象变成被**蒸馏/打包**的商品；③ Salesforce AIforce 直接命名「**Enterprise AI Harness**」（数据+业务知识+工作流+控制的可组合架构供 agent 执行，来源 [AI Agent Store 日报 2026-09-22](https://aiagentstore.ai/ai-agent-news/daily/2026-09-22)），使 harness 作为「企业级可组合执行面」进入主流厂商叙事——与 [[AI智能体分权治理]]「角色分层 + 变更治理闭环」互为表里：harness 商品化后，谁控制其内部约束成为治理问题。

## 参考实例

- `[[Proteus]]`：本概念的实现载体（MIT，v0.3.0）。
- 内置 `minimal` harness 可离线复现「装 review:notes 扰动 → notes 单位数变化可测」的最小演示。

## 相关

- `[[Proteus]]` - 实体
- `[[自进化测量标尺]]` - 怎么测「进化了没有」
- `[[Agent]]` - harness 作为 2026 竞争焦点的语境
- `[[自愈闭环设计]]` - 另一种「agent 改自己」的安全通道
- `[[AI智能体工程方法论]]` - 工程侧原则对照
- `[[每日AI素材-2026-09-18-摘要]]` - 自维护 Wiki 再印证 + harness 可靠性失败面量化
- `[[每日AI素材-2026-09-19-摘要]]` - harness 成为组件级研究对象 + OpenWiki 第三次印证
- `[[每日AI素材-2026-09-19-晚-摘要]]` - harness 成本度量(HarnessTax) + 边缘自动化模型 + 多模态推理优化 + AI 治理合规
- `[[每日AI素材-2026-09-20-摘要]]` - harness 设计层实证(2609.20804/SoL-Pi/C2C) + 世界模型 + 概率即输出 + 医疗影像开源
- `[[每日AI素材-2026-09-20-晚-摘要]]` - coding-agent/harness 安全审计霸榜延续 + computer-use/Generative UI + Agents-Flex 把 LLM Wiki 列为内置 + 推理路由经济学/REALM 记忆再巩固补强成本度量
- `[[每日AI素材-2026-09-21-摘要]]` - agent 技能从代码库挖掘成共识 + harness 建议随上下文窗口失效 + Skills over MCP 定稿 + coding agent 外泄威胁模型 + Paper2Agent 论文即可执行
- `[[每日AI素材-2026-09-22-摘要]]` - harness 被蒸馏(Harness-Zero) + agent/harness 运行时霸榜 + Salesforce Enterprise AI Harness 商品化
- `[[LLM-Wiki-vs-RAG]]` - 知识库防腐坏：编译停摆则网络腐坏
- `[[自生长知识库实战-苍何]]` - 自生长知识库的方法论原型
