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
updated: 2026-09-16
sources:
  - "[[raw/20-Tech/RAG检索增强生成]]"
  - "[[raw/20-Tech/为什么转岗AI大模型应用]]"
  - "[[raw/00-Inbox/每日AI素材-2026-09-14]]"
  - "[[raw/00-Inbox/每日AI素材-2026-09-15]]"
  - "[[raw/00-Inbox/每日AI素材-2026-09-16]]"
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

## 相关链接

- [[RAG]] - Agent 记忆层的基础
- [[AI应用开发]] - 应用层核心组件之一
- [[每日AI素材-2026-09-14-摘要]] - 2026-09-14 agent 工具化冲榜趋势信号
- [[每日AI素材-2026-09-15-摘要]] - 2026-09-15 agent 工具化 + RSI 热点趋势信号
- [[每日AI素材-2026-09-16-摘要]] - 2026-09-16 自维护 Wiki 开源印证 + agent harness 工具 + Plan Injection 安全议题
- [[AI智能体分权治理]] - agent-skills 注册表的安全校验本质
- [[AI智能体工程方法论]] - RSI / 自改进闭环 / agent 工具接口 / eval 盲点
- [[Proteus]] - harness 自进化实验框架（2026-09-16 精读）
- [[Harness自进化]] - 演化对象从权重到 harness
- [[自进化测量标尺]] - 测「进化了没有」的三把尺