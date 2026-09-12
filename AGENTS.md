# LLM Wiki Schema

> 本文件是 LLM 与人类协作维护本知识库的行为规范（“合同”）。
> LLM 在 ingest / query / lint / publish / refresh 等任何操作前，**必须先读取本文件**。
> 人类也可根据实际体验逐步修改本文件，使其持续演进。

---

## 1. 目录结构与权限

知识库采用四层架构，各层职责清晰，权限严格。

| 分区        | 用途                           | 权限规则                                       |
| ----------- | ------------------------------ | ---------------------------------------------- |
| `raw/`      | 原始素材（人类写入）           | **只读**。LLM 绝不修改、删除、重命名任何文件   |
| `wiki/`     | 编译产物（LLM 读写）           | 读写。LLM 负责创建和维护所有页面，人类只浏览   |
| `output/`   | 成品输出（协作区）             | 读写。LLM 生成草稿，人类审核定稿               |
| `skills/`   | Agent Skill 工作流详细规则     | 只读。按需加载                                 |

### raw/ 子目录

| 目录               | 用途                             |
| ------------------ | -------------------------------- |
| `raw/00-Inbox/`    | 快速收集箱（网页剪藏、临时笔记） |
| `raw/01-Daily/`    | 每日笔记（启用 Daily notes）     |
| `raw/10-Work/`     | 工作相关素材                     |
| `raw/20-Tech/`     | 技术知识素材                     |
| `raw/30-Life/`     | 生活日常素材                     |
| `raw/80-Templates/`| 笔记模板                         |
| `raw/90-Attachments/` | 图片、PDF 等二进制附件        |
| `raw/95-Archive/`  | 归档区                           |

### wiki/ 子目录

| 目录               | 用途                       |
| ------------------ | -------------------------- |
| `wiki/index.md`    | 全局索引（内容导向入口）   |
| `wiki/log.md`      | 操作日志（时间导向记录）   |
| `wiki/lifecycle.md`| 生命周期数据（可插拔）     |
| `wiki/entities/`   | 实体页（工具、框架、人物） |
| `wiki/concepts/`   | 概念页（设计模式、方法论） |
| `wiki/summaries/`  | 素材摘要页                 |
| `wiki/comparisons/`| 对比分析页                 |
| `wiki/synthesis/`  | 综合分析页                 |
| `wiki/logs/`       | 按年归档的历史 log         |

### output/ 子目录

| 目录               | 用途             |
| ------------------ | ---------------- |
| `output/posts/`    | 博客文章         |
| `output/reports/`  | 研究报告         |
| `output/slides/`   | 演示文稿（Marp） |
| `output/tutorials/`| 教程指南         |
| `output/newsletters/`| 知识简报       |

> **最重要的规则：raw 只读，wiki 读写，绝不修改原始素材。**

---

## 2. 页面格式规范

每个 wiki 页面**必须**包含以下 YAML frontmatter（`---` 包裹于文件头部）：

```yaml
---
title: 页面标题
aliases:
  - 别名1
  - 别名2
tags:
  - 领域标签
  - 类型标签
category: entities   # entities | concepts | summaries | comparisons | synthesis
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources:
  - "[[raw/路径/源文件名]]"
description: 一句话摘要，不超过100字
---
```

规范要求：

- `category`：必填，取值必须严格落在这五类：`entities`、`concepts`、`summaries`、`comparisons`、`synthesis`
- `created` / `updated`：使用 `YYYY-MM-DD` 格式
- `sources`：素材来源必须用 `[[双链]]` 指向 raw 层
- `description`：不超过 100 字的一句话摘要，用于 Dataview 查询展示
- 页面内通过 `[[双链]]` 与其他页面交叉引用

---

## 3. 标签体系

从三个维度为每个页面打标签，**保持一致性**：

### 领域标签（Dimension: Domain）
`AI`、`软件工程`、`数据库`、`架构`、`知识管理`、`机器学习`、`云原生`、`DevOps`、`编程语言`、`前端`、`后端`、`产品`、`沟通`、`效率`

### 类型标签（Dimension: Type）
`工具`、`框架`、`模式`、`概念`、`人物`、`方法论`、`对比分析`、`综合洞察`、`材料摘要`

### 成熟度标签（可选，Dimension: Maturity）
`成熟`、`新兴`、`实验性`、`已废弃`

> 标签语言（中文/英文）取决于个人偏好，选一种并保持一致。本库默认使用中文标签。

---

## 4. 工作流程定义

### Ingest（摄入：消化一篇素材）

1. 读取素材全文，提取关键信息
2. 向用户汇报关键发现，确认重点
3. 创建摘要页到 `wiki/summaries/`
4. 创建或更新实体页（`wiki/entities/`）、概念页（`wiki/concepts/`）
5. 在页面之间建立 `[[双链]]` 交叉引用
6. 更新 `wiki/index.md` 与 `wiki/log.md`

### Query（查询：基于已编译知识回答）

1. 读取 `wiki/index.md` 定位相关页面
2. 阅读相关页面内容
3. 综合各页面合成答案
4. 在回答中提供 `[[引用]]` 注明来源
5. 询问用户是否将优质答案存档（对比分析存 `comparisons/`，综合洞察存 `synthesis/`）

### Lint（健康检查：定期维护）

检查以下项目：
- 矛盾检测：不同页面关于同一主题的描述是否冲突
- 孤立页面：无任何 `[[入链]]` 的页面
- 缺失概念页：被引用但尚未创建的概念
- 过时声明：明显过时的内容
- 交叉引用完整性：断链
- 生成修复建议，用户确认后修复

### Publish（发布：产出成品）

1. 确认输出类型（post/report/slides/tutorial/newsletter）和受众
2. 从 `wiki/index.md` 定位相关 wiki 页面
3. 阅读来源页面
4. 生成草稿到 `output/` 对应子目录
5. **将 `[[双链]]` 转为标准 Markdown 链接**（成品必须独立可读）
6. 用户审核修改
7. 记录到 `wiki/log.md`

### Refresh（联网重校验：应对过时）

1. 针对怀疑过时的主题，联网检索最新信息
2. 对比新旧内容差异
3. 用户确认后，将新信息存为 raw 素材
4. 重新 ingest 更新 wiki