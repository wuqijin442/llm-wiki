---
title: Ollama
aliases:
  - Ollama 本地推理
tags:
  - AI
  - 工具
  - 实体
category: entities
created: 2026-09-12
updated: 2026-09-12
sources:
  - "[[raw/20-Tech/记忆-DGX-AI智能体与RAG工程]]"
description: 本地大模型推理运行时，承载本地 LLM 供 agent 网关 bypass 直连调用。
status: seed
---

# Ollama

## 概述

本地大模型推理运行时，用于在私有大模型环境运行可下载的开源模型（如 qwen3 系）。本项目将其作为本地 AI 智能体的推理底座。

## 使用要点

- API 暴露于本地端口（如 /api/tags 列举已装模型、/api/generate 生成）。
- `num_ctx` 需显式配置：调用方（如 Dify）侧默认 8192；harness 层可更高（16384）。
- 直连生成时按用途区分输出预算（RAG 小步长、整文件生成加大至 4096~8192），避免中途截断。
- 有条件时可经 bridge 由 agent 直连绕过业务网关，减少耦合但须守住安全边界。

## 相关
- [[RAG]] - 本地模型常结合向量库
- [[AI智能体分权治理]] - agent 底层推理
- [[本地优先AI创作]] - 本地推理的另一应用