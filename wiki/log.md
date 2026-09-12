# Wiki Log

> 操作日志（时间导向记录）。每次操作追加一条，最近 30 天在此展示，更早记录归档至 `wiki/logs/`。

## [2026-09-12] ingest | AI大模型应用学习笔记（批量 4 篇）
- 来源：`raw/20-Tech/RAG检索增强生成.md`
- 新建：[[RAG检索增强生成学习笔记]]（摘要页）、[[Milvus]]（实体页）
- 更新：[[RAG]]（新增检索优化与记忆层章节、sources）
- 来源：`raw/20-Tech/为什么转岗AI大模型应用.md`
- 新建：[[为什么转岗AI大模型应用]]（摘要页）、[[AI应用开发]]、[[Agent]]（概念页）
- 来源：`raw/10-Work/langgenius-dify.md`
- 新建：[[Dify]]（实体页）
- 来源：`raw/10-Work/GitHub-AI周报-2026-08-09.md`
- 新建：[[GitHub-AI周报2026-08-09]]（摘要页）
- 更新：index.md、log.md

## [2026-09-12] ingest | Obsidian + LLM Wiki 实操指南
- 来源：`raw/00-Inbox/Obsidian-LLM-Wiki实操指南.md`
- 新建：[[Obsidian-LLM-Wiki实操指南-摘要]]（摘要页）
- 新建：[[Obsidian]]（实体页）
- 新建：[[RAG]]、[[双链网络]]（概念页）
- 新建：[[LLM-Wiki-vs-RAG]]（对比分析页）
- 更新：index.md、log.md

## [2026-09-12] init | 初始化知识库

- 操作：搭建四层目录结构（raw / wiki / output / skills）
- 新建：`AGENTS.md`（Schema 规范）
- 新建：`skills/llm-wiki/SKILL.md`（Skill 工作流）
- 新建：`raw/80-Templates/` 下 6 个笔记模板
- 新建：`wiki/index.md`、`wiki/log.md`
- 配置：Obsidian 附件路径、每日笔记、模板插件
- 新建：`README.md`（使用说明）

---

## 日志格式参考

### [YYYY-MM-DD] ingest | 素材标题
- 来源：`raw/路径/文件名`
- 新建：[[页面1]]、[[页面2]]
- 更新：[[页面3]]（新增xxx章节）
- 更新：index.md

### [YYYY-MM-DD] query | 查询问题
- 查阅：[[页面1]]、[[页面2]]
- 产出：[[对比分析页]]（已存入 wiki）

### [YYYY-MM-DD] lint | 健康检查
- 发现矛盾：[[页面A]] 与 [[页面B]] 关于xxx的描述不一致
- 孤立页面：[[页面C]]（无入链）
- 缺失概念页：建议创建 [[概念X]]
- 已修复：N 项 | 待修复：M 项

### [YYYY-MM-DD] publish | 成品标题
- 类型：post | report | slides | tutorial | newsletter
- 输出：`output/posts/文件名.md`
- 来源：[[wiki 页面1]]、[[wiki 页面2]]...
- 状态：草稿 | 已审核