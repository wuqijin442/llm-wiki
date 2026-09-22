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
updated: 2026-09-22
sources:
  - "[[raw/00-Inbox/Obsidian-LLM-Wiki实操指南]]"
  - "[[raw/20-Tech/RAG检索增强生成]]"
  - "[[raw/00-Inbox/每日AI素材-2026-09-21]]"
  - "[[raw/00-Inbox/refresh-verify-2026-09-22]]"
  - "[[raw/00-Inbox/每日AI素材-2026-09-22]]"
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

## 知识即可执行：论文即 MCP Agent（补强，2026-09-21；2026-09-22 已核实）

[[每日AI素材-2026-09-21-摘要]] 信号五：Stanford **Paper2Agent**（Nature 2026-09-16，DOI [10.1038/s41586-026-11044-y](https://www.nature.com/articles/s41586-026-11044-y)，作者 Jiacheng Miao / Joe R. Davis / Yaohui Zhang / Jonathan K. Pritchard / James Zou）把一篇论文转成 MCP 服务器（Paper2MCP）再接兼容 agent，形成「虚拟通讯作者」（virtual corresponding author）——自动搭环境 / 提取工具 / 测试-修复。

**已核实数字（2026-09-22 经 Nature 一手 + AI Weekly 二级交叉确认）**：
- 规模：**100 篇计算生物学论文 → 74 篇做成可用 agent**，599 工具提议、**593 通过自动校验**。
- AlphaGenome showcase：22 工具、~45 min、$14；tutorial-derived 查询 **98.7%**（vs Claude+Repo 82.7%、Biomni 37.3%），novel 查询 **100%**（vs 78.7% / 56.0%）。
- 多 agent 协作：AlphaGenome + MPRA-scCRISPRi + Perturb-seq 链锁定 **GPR137** 为银屑病变异 rs887314 可能因果基因（Spearman 0.613）。

这与「RAG vs LLM Wiki」的边界互补：传统 RAG 是「检索片段拼上下文」，Paper2Agent 走到「把方法封装为可执行的 MCP 工具 + 资源 + 提示」。知识从被动检索对象升级为可调用、可协作的活性组件——与 [[双环知识飞轮]] / [[自生长知识库实战-苍何]] 的「知识库自生长」同源，但前者是论文级、后者是库级。

> **caveat（补强）**：失败主因 = 缺可执行代码 / 缺数据或模型工件 / 环境失败 / 脚本不可泛化；论文未报告失败原因。执行通过验证 ≠ 科学结论正确，假设提出仍须人类主导。来源与细节见 [[每日AI素材-2026-09-21-摘要]]（信号五）+ [[refresh-verify-2026-09-22]]（核验记录）。同日 WebSearch 补充：面壁 MiniCPM5-2B / 微软 FastContext 也指向「知识/上下文检索下沉到专用小模型」的成本工程切面。

## 记忆成本前沿与 RSI 医疗化（补强，2026-09-22）

[[每日AI素材-2026-09-22-摘要]] 信号四、信号三把本页记忆层推进到「可测的 cost/accuracy 前沿」，并把 RSI（递归自我改进）补到医疗领域：

- **记忆 accuracy vs cost Pareto 前沿**：DolphinBench 绘制 agent 记忆系统的 accuracy–cost 前沿，并批评旧记忆基准用「对话 QA」问错了问题（奖励回忆人说的话，而非 agent 记忆真实负载）；同期 Jev-Mem 提出 System-One 式快速记忆控制器（来源：[Daily AI Brief 2026-09-22](https://wire.rundatarun.io/briefs/2026-09-22)）。记忆层从「有没有」进入「准不准/贵不贵」的可测阶段——与 [[Harness自进化]]「memory 是被演化、也需被约束的 surface」同构。
- **小模型语义熵动态路由**：WebSearch 热点指出 3B 以下端侧小模型 Token 级熵在 91% 数据集失效，用多采样聚类的**语义熵**恢复置信度并指导动态模型路由（SmolLM→Phi-3.5），综合准确率最高 +50 个百分点（来源：[SegmentFault AI 日报 2026-09-22](https://segmentfault.com/a/1190000048311861)）。
- **RSI 医疗化**：[2609.24838 MedRSI: Recursive Self-Improvement for Medical Agents](https://arxiv.org/abs/2609.24838)（Junde Wu et al.）把递归自我改进用于医疗 agent，通过临床对齐的自演化持续改进；与 [[Agent]] 09-21 信号「技能从代码库挖掘」、本页「知识即可执行（Paper2Agent）」构成 RSI + 记忆 + 可执行知识的三联演进。

> 待核实：DolphinBench / 语义熵路由 / MedRSI 均来自媒体日报或仅据 arXiv 标题，未对照一手论文。

## 参考实例

NotebookLM 是典型 RAG 模式：扔文件即可问答，但知识不积累。

## 相关链接

- [[LLM-Wiki-vs-RAG]] - 两种范式对比
- [[Obsidian-LLM-Wiki实操指南-摘要]] - 上下文出处
- [[RAG检索增强生成学习笔记]] - 企业落地方案深化
- [[Milvus]] - 生产级向量数据库
- [[Agent]] - 记忆层是 RAG 之上的关键升级
- [[每日AI素材-2026-09-21-摘要]] - 论文即可执行 agent（Paper2Agent）/ 知识即可执行
- [[每日AI素材-2026-09-22-摘要]] - 记忆成本前沿(DolphinBench) + 语义熵动态路由 + MedRSI 医疗化 RSI