---
title: RAG
aliases:
  - 检索增强生成
  - Retrieval-Augmented Generation
tags:
  - AI
  - 概念
  - 机器学习
category: concepts
created: 2026-09-12
updated: 2026-09-12
sources:
  - "[[raw/00-Inbox/Obsidian-LLM-Wiki实操指南]]"
  - "[[raw/20-Tech/RAG检索增强生成]]"
description: 检索增强生成（Retrieval-Augmented Generation），LLM 结合外部文档检索的主流交互方式。
status: growing
---

# RAG

## 定义

检索增强生成（Retrieval-Augmented Generation）：模型在回答前先从外部文档库检索相关内容，再结合检索结果生成答案。

## 核心思想

先用向量索引把文档语义化，回答时查询检索相关片段，拼装成上下文交给 LLM 生成。解决 LLM 知识过时与缺乏私有数据的问题。

## 应用场景

- 企业私域知识问答（NotebookLM、各类知识库问答系统）
- 需要引用最新或专有资料时的回答

## 与传统方案的对比

| 维度 | 传统 RAG | LLM Wiki |
|---|---|---|
| 知识存储 | 原始文档 + 向量索引 | 结构化、互相链接的 Markdown 页面 |
| 查询方式 | 每次从零检索、拼凑 | 基于已编译的知识综合回答 |
| 知识积累 | 无 | 有（持续复利） |
| 交叉引用 | 无 | 自动维护的 `[[双链]]` 网络 |
| 矛盾处理 | 不感知 | 主动标注 |

## 参考实例

NotebookLM 是典型 RAG 模式：扔文件即可问答，但知识不积累。

## 相关链接

- [[LLM-Wiki-vs-RAG]] - 两种范式对比
- [[Obsidian-LLM-Wiki实操指南-摘要]] - 上下文出处
- [[RAG检索增强生成学习笔记]] - 企业落地方案深化
- [[Milvus]] - 生产级向量数据库
- [[Agent]] - 记忆层是 RAG 之上的关键升级