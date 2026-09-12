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
updated: 2026-09-12
sources:
  - "[[raw/20-Tech/RAG检索增强生成]]"
  - "[[raw/20-Tech/为什么转岗AI大模型应用]]"
description: AI 智能体：能调工具、能续跑的自主任务执行体，RAG 之上记忆层是其关键组件。
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

## 相关链接

- [[RAG]] - Agent 记忆层的基础
- [[AI应用开发]] - 应用层核心组件之一