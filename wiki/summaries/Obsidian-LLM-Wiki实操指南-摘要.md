---
title: Obsidian-LLM-Wiki实操指南摘要
aliases:
  - LLM Wiki 实操指南
  - 自增长知识库搭建
tags:
  - 知识管理
  - AI
  - 工具
  - 材料摘要
category: summaries
created: 2026-09-12
updated: 2026-09-12
sources:
  - "[[raw/00-Inbox/Obsidian-LLM-Wiki实操指南]]"
description: 用 Obsidian + LLM 搭建自增长知识库的完整实操指南，含四层架构与五种工作流。
status: seed
---

# Obsidian-LLM-Wiki实操指南摘要

> 来源素材：[[raw/00-Inbox/Obsidian-LLM-Wiki实操指南]]

## 核心要点

1. **模式本质**：Obsidian 当"IDE"，LLM 当"程序员"，wiki 当"代码库"。人类浏览审阅，LLM 编写维护。
2. **独特价值**：相比传统 RAG，知识能持续积累复利（每次 ingest/query 让 wiki 更丰富）。
3. **四层架构**：raw（只读）/ wiki（读写）/ output（协作）/ skills（规则）。
4. **铁律**：raw 只读，LLM 绝不修改原始素材。
5. **五种工作流**：Ingest / Query / Lint / Publish / Refresh。
6. **上手成本低**：约 30 分钟即可跑通第一次 ingest。

## 详细摘要

### 架构与目录
知识库采用四层架构。`raw/` 是人类领地（收集箱、日记、工作、技术、生活、模板、附件、归档）；`wiki/` 是 LLM 领地（索引、日志、实体、概念、摘要、对比、综合）；`output/` 是协作区（博客、报告、幻灯片、教程、简报）；`skills/` 承载工作流规则。obsidian 的 `[[双链]]` 是 wiki 交叉引用的基础。

### 配置要点
附件路径设为 `raw/90-Attachments`，每日笔记设为 `raw/01-Daily`（格式 YYYY-MM-DD），模板文件夹设为 `raw/80-Templates`。推荐插件：Dataview（对 frontmatter 做类 SQL 查询）、Web Clipper（网页一键剪藏到 Inbox）、可选 Excalidraw 和 Marp。

### Schema（灵魂）
Schema 文件（AGENTS.md / CLAUDE.md）定义四部分：目录结构与权限、页面格式规范（标准 frontmatter）、标签体系（领域/类型/成熟度三维度）、工作流程定义。Schema 是人与 LLM 共同演进的"合同"。

### 规模演进
<300 页单 index 够用；300-1000 页拆分散索引；1000+ 引入语义检索。log 按年归档即可。

## 个人理解与启发

- 这套模式的关键洞见是把"检索"换成"编译"——与传统 RAG 的每次重推相比，wiki 是在做一次性的知识投资。
- Schema 的迭代心态很重要：先最小可用，用起来再改。

## 待核实

- 跨设备多人协作时的冲突如何处理，文章的 git 方案是否够用，可后续验证。

## 关联页面

- [[Obsidian]] - 承载本模式的笔记工具
- [[RAG]] - 与 LLM Wiki 形成对比的知识处理范式
- [[自生长知识库实战-苍何]] - 同主题的另一份实践（含三种搭建路径对比）
- [[双链网络]] - 交叉引用的基础
- [[LLM-Wiki-vs-RAG]] - 模式对比分析