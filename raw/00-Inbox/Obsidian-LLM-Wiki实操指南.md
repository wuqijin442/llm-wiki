---
title: Obsidian + LLM Wiki 自增长知识库实操指南
tags:
  - 知识管理
  - AI
source: https://blog.csdn.net/Python_cocola/article/details/161936333
---

> 来源：CSDN 博客《实操篇：怎么用 Obsidian + LLM 把这套模式跑起来》
> 作者：Python_cocola（2026）

# Obsidian + LLM Wiki 自增长知识库实操指南

## 核心理念

用 LLM 维护一个结构化、互相链接的 Markdown 知识库（wiki）。

比喻（Karpathy）：**Obsidian 是"IDE"，LLM 是"程序员"，wiki 是"代码库"**。
人类在 Obsidian 里浏览和审阅，LLM 负责编写和维护，知识持续复利。

## 与传统 RAG 的区别

| 维度 | 传统 RAG | LLM Wiki |
|---|---|---|
| 知识存储 | 原始文档 + 向量索引 | 结构化、互相链接的 Markdown 页面 |
| 查询方式 | 每次从零检索、拼凑 | 基于已编译的知识综合回答 |
| 知识积累 | 无——每次重新推导 | 有——每次操作都让 wiki 更丰富 |
| 交叉引用 | 无 | 自动维护的 [[双链]] 网络 |
| 矛盾处理 | 不感知 | 主动标注、注明来源 |

## 四层架构

- `raw/`（人类写入，LLM 只读）：00-Inbox / 01-Daily / 10-Work / 20-Tech / 30-Life / 80-Templates / 90-Attachments / 95-Archive
- `wiki/`（LLM 读写）：index.md、log.md、entities/、concepts/、summaries/、comparisons/、synthesis/
- `output/`（LLM 生成，人类审核）：posts / reports / slides / tutorials / newsletters
- `skills/`：Agent Skill 工作流规则

关键：**raw 只读，wiki 读写，绝不修改原始素材**。

## 搭建步骤（约 30 分钟）

1. 安装 Obsidian，创建 vault
2. 搭建四层目录结构
3. 配置 Obsidian（附件路径 → raw/90-Attachments；每日笔记 → raw/01-Daily；模板 → raw/80-Templates）
4. 安装插件：Dataview（强烈推荐）、Web Clipper（强烈推荐）、Excalidraw（可选）、Marp Slides（可选）
5. 编写 Schema 文件（AGENTS.md / CLAUDE.md）
6. 初始化 wiki/index.md 与 wiki/log.md
7. 第一次 ingest

## 页面格式规范

每个页面含 YAML frontmatter：
title / aliases / tags / category（entities|concepts|summaries|comparisons|synthesis）/ created / updated / sources / description（≤100字）

标签体系三维度：领域标签、类型标签、成熟度标签。

## 核心工作流

- **Ingest**：读素材 → 提取关键信息 → 与用户讨论 → 建摘要页 → 建/更实体/概念页 → 维护双链 → 更新 index & log
- **Query**：读 index 定位 → 读相关页 → 综合答案 → 提供引用 → 询问存档
- **Lint**：矛盾检测 → 孤立页面 → 缺失概念页 → 过时声明 → 交叉引用完整性 → 修复建议
- **Publish**：确认类型受众 → 定位 wiki 页 → 读来源页 → 生成草稿到 output → 双链转标准链接 → 用户审核 → 记 log
- **Refresh**：联网检索 → 对比差异 → 存为 raw → 重新 ingest

## 规模增长策略

| 阶段 | 页面数 | 状态 |
|---|---|---|
| 起步期 | < 300 | 单文件 index.md 够用 |
| 中期 | 300-1000 | index 拆分为分类子索引 |
| 后期 | 1000+ | 引入语义搜索，index 降级为辅助导航；log 按年归档 |

## 关键心态

不需要一次消化所有存量笔记。从今天开始，每次碰到新素材就 ingest 一篇，wiki 会自然生长。
知识会在持续复利。开始养你的知识花园吧。