---
title: Milvus
aliases:
  - 米兰达
tags:
  - 数据库
  - 向量数据库
  - 工具
  - 实体
category: entities
created: 2026-09-12
updated: 2026-09-12
sources:
  - "[[raw/20-Tech/RAG检索增强生成]]"
description: 生产首选的分布式向量数据库，面向企业级大规模向量检索场景。
status: seed
---

# Milvus

## 概述

面向生产环境的分布式向量数据库，是企业级大规模 [[RAG]] 检索的首选方案。

## 背景

RAG 向量数据库选型中各库定位不同：FAISS（本地轻量）、Chroma（快速原型）、Milvus（生产首选）、PGVector（后端友好）。

## 关键特性

- **分布式**：支持海量向量数据水平扩展
- **生产级**：适合企业级大库检索场景

## 使用方式

- 用于企业级知识库（如 e1-enterprise-kb）的向量存储与相似度检索

## 优缺点

### 优点
生产可用、规模化、生态成熟

### 缺点
相对 FAISS/Chroma 部署与运维成本更高

## 相关链接

- [[RAG]] - 向量数据库是其核心组件
- [[RAG检索增强生成学习笔记]] - 选型对比上下文