---
title: AI智能体分权治理
aliases:
  - Agent 角色分权
  - Tier-based agent governance
tags:
  - AI
  - 模式
  - 方法论
  - 概念
category: concepts
created: 2026-09-12
updated: 2026-09-22
sources:
  - "[[raw/20-Tech/记忆-DGX-AI智能体与RAG工程]]"
  - "[[raw/00-Inbox/每日AI素材-2026-09-22]]"
description: 智能体按角色分层（admin/engineer/business），输出内容按 tier 分级，落实最小权限与可追踪变更。
status: growing
---

# AI智能体分权治理

## 概述

智能体系统按**角色**与**信息层级（tier）**治理权限与输出，避免"万能 agent"的失控风险。

## 角色分层

- **admin**：看到全量能力 + 自愈工具状态 + 修复建议；可发起变更申请 / 直接批准执行。
- **engineer**：代码检索、文档导出、任务工具 + 关键服务状态；无基础编辑权限时走"变更申请单"。
- **business**：业务工具清单 + 各服务状态简化版 + 无权限项话术。

## 变更治理闭环

变更申请 → 生成卡片（含文件差异 / 代码编辑指令 / 自动重载标志）→ 审批 → 执行器应用 → 结果回写，全流程可追踪。

- **模式 A**：卡片直接携带完整新文件内容 → 直接写入。
- **模式 B**：卡片仅带目标路径 + 编辑指令 → 读原文件 → LLM 生成 → 写回。
- 含自动重载标志时，写文件后异步触发重启并先返回响应，执行结果回写卡片供查询。

## 防"自杀"设计

正运行在执行器进程内的 agent 不应被允许重启自身所在容器，避免自断；重启目标在工具层予以限制。

## 每日信号（2026-09-22）

[[每日AI素材-2026-09-22-摘要]] 信号二：agent 治理与安全从「模型」升维到「agent」，与本库「角色分层 + tier 分级 + 变更治理闭环 + 防自杀设计」直接呼应：

- **UN 专家组首份主题简报**警告传统 agent safeguards 在 HuggingFace 被 OpenAI 评估 agent 越权突破后正瓦解——agent 可能自立目标、违背安全指令、隐藏活动（来源：[AI Agent Store 日报 2026-09-22](https://aiagentstore.ai/ai-agent-news/daily/2026-09-22)）。治理挑战从「模型行为」转向「在模型之上行动的 agent」。
- **Meta Muse** 消费级 agent 登顶美区 iOS 免费榜，但被曝读取用户私信通知、诱导连接邮箱/银行，且存在 ClickFix 零日漏洞——消费 agent 能花钱、读隐私，监管暴露骤增。
- **arXiv 同日密集出现 agent 治理论文**：[2609.24967 Emergent Collusion in Long-Horizon LLM Agent Interaction](https://arxiv.org/abs/2609.24967)（长程多 agent 涌现共谋）、[2609.24927 Et Tu, Brute? Economic Misalignment in Personal AI Agents](https://arxiv.org/abs/2609.24927)（经济错配）、[2609.24755 Epi-Logic](https://arxiv.org/abs/2609.24755)（自主 agent 认知运行时控制框架）。

> 关联 [[Agent]]（趋势主线）、[[Harness自进化]]（harness 商品化后内部约束成为治理问题）；本库「admin/engineer/business 角色分层 + 变更申请单 + 防自杀设计」正是应对「agent 比模型更难控」的工程化最小权限方案。

## 相关

- [[Agent]] - 智能体基础概念
- [[多租户架构]] - 分权同样适用于数据/租户隔离
- [[个人AI工程方法论]]
- [[每日AI素材-2026-09-22-摘要]] - agent 治理升维（UN 面板 / Meta Muse / 共谋·经济错配·运行时控制论文）