---
title: LLM-Wiki-vs-RAG
aliases:
  - LLM Wiki 与传统 RAG 对比
tags:
  - AI
  - 知识管理
  - 对比分析
category: comparisons
created: 2026-09-12
updated: 2026-09-12
sources:
  - "[[raw/00-Inbox/Obsidian-LLM-Wiki实操指南]]"
description: LLM Wiki 模式与传统 RAG 在知识组织、查询、积累与矛盾处理上的对比分析。
---

# LLM-Wiki-vs-RAG

## 对比维度

知识存储、查询方式、知识积累、交叉引用、矛盾处理。

## 对比表

| 维度 | 传统 RAG | LLM Wiki |
|---|---|---|
| 知识存储 | 原始文档 + 向量索引 | 结构化、互相链接的 Markdown 页面 |
| 查询方式 | 每次从零检索、拼凑 | 基于已编译的知识综合回答 |
| 知识积累 | 无——每次重新推导 | 有——每次操作都让 wiki 更丰富 |
| 交叉引用 | 无 | 自动维护的 `[[双链]]` 网络 |
| 矛盾处理 | 不感知 | 主动标注、注明来源 |

## 分析结论

两者的根本差异在"检索"与"编译"：RAG 每次从零推理，无状态、无积累；LLM Wiki 把知识编译成结构化页面，每次操作都在积累。

## 选择建议

> 判断标准：这个知识以后还会用到吗？
> 会 → **LLM Wiki**；不会 → **NotebookLM（RAG）** 问完即走。

## 参考来源

- [[Obsidian-LLM-Wiki实操指南-摘要]]
- [[RAG]]
- [[Obsidian]]