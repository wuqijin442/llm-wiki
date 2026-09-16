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
updated: 2026-09-15
sources:
  - "[[raw/20-Tech/RAG检索增强生成]]"
  - "[[raw/20-Tech/为什么转岗AI大模型应用]]"
  - "[[raw/00-Inbox/每日AI素材-2026-09-14]]"
  - "[[raw/00-Inbox/每日AI素材-2026-09-15]]"
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

## 相关链接

- [[RAG]] - Agent 记忆层的基础
- [[AI应用开发]] - 应用层核心组件之一
- [[每日AI素材-2026-09-14-摘要]] - 2026-09-14 agent 工具化冲榜趋势信号
- [[每日AI素材-2026-09-15-摘要]] - 2026-09-15 agent 工具化 + RSI 热点趋势信号
- [[AI智能体分权治理]] - agent-skills 注册表的安全校验本质
- [[AI智能体工程方法论]] - RSI / 自改进闭环 / agent 工具接口 / eval 盲点