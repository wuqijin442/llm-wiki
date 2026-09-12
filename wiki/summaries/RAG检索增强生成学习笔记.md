---
title: RAG检索增强生成学习笔记摘要
aliases:
  - RAG 企业落地
  - 04-RAG学习笔记
tags:
  - AI
  - RAG
  - 软件工程
  - 材料摘要
category: summaries
created: 2026-09-12
updated: 2026-09-12
sources:
  - "[[raw/20-Tech/RAG检索增强生成]]"
description: AI 大模型应用学习路线第4篇：RAG 全链路、分块/向量库/检索优化，以及 2026 新增的 Agent 记忆层五层栈。
---

# RAG检索增强生成学习笔记摘要

> 来源素材：[[raw/20-Tech/RAG检索增强生成]]（来自 `ai-llm-app-roadmap/docs/04`）

## 核心要点

1. **RAG 本质**：用「私有/实时知识」增强模型，压制幻觉、解决知识滞后，是企业落地第一刚需。
2. **Naive RAG 全链路**：文档加载 → 清洗分块 → 向量化 → 入向量库 → query 检索 → 拼接 Prompt → LLM 生成（带引用）。
3. **分块策略三型**：固定长度（递归字符）、语义分块、层级分块（父子块）；经验块大小 300–800 token，重叠 10–20%。
4. **中文 Embedding**：BGE（智源）、m3e、text-embedding-3、text-embedding-v3；本地练习用 bge-small-zh / m3e-base。
5. **2026 检索优化必学**：混合检索（向量+BM25）、重排序（Rerank）、上下文压缩、多轮记忆、幻觉检测。
6. **2026 新增「记忆层」**：五层记忆栈 + 检索经济学 + 记忆晋升门控（防记忆投毒）。

## 详细摘要

### 向量数据库选型
| 库 | 定位 | 适用 |
|---|---|---|
| FAISS | 本地内存，极致轻量 | 测试 / 小数据 |
| Chroma | 轻量易用 | 快速原型 |
| Milvus | 生产首选，分布式 | 企业级大库 |
| PGVector | Postgres 向量扩展 | 后端友好，复用现有 DB |

### 记忆层五层栈（2026 新增）
1. **Working Memory**（Redis 毫秒级）：当前对话上下文
2. **Short-term Memory**（TTL）：会话历史
3. **Long-term Memory**（Postgres）：用户/租户长期事实
4. **Semantic Memory**（向量+BM25+rerank <200ms）：语义知识
5. **Episodic Memory**（ClickHouse <500ms）：事件时序

关键数据：选择性检索可 **91% 延迟降 + 90% token 省**；记忆晋升门控（session→user→tenant→global）防 guardrail drift 与记忆投毒（对标 OWASP ASI06）。

### 常见坑
块太大→检索不精准；块太小→上下文断裂；只向量不重排→噪声多；不展示来源→无法信任；不做评测集→不可证伪。

## 个人理解与启发

- RAG 的工程难点不在接入，而在分块与检索精排——这部分最依赖经验。
- 「记忆晋升门控」是 RAG 到 Agent 的关键升级，关系安全与合规。

## 关联页面

- [[RAG]] - 概念页（本素材是其深化）
- [[Milvus]] - 生产级向量数据库
- [[AI应用开发]] - 学习路线上下文
- [[Agent]] - 记忆层是其关键组件