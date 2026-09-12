# llm-wiki

Obsidian + LLM 自增长知识库（LLM Wiki 模式）。参考 [CSDN 实操指南](https://blog.csdn.net/Python_cocola/article/details/161936333) 搭建。

> 比喻（Karpathy）：**Obsidian 是"IDE"，LLM 是"程序员"，wiki 是"代码库"。**
> 你在 Obsidian 里浏览和审阅，LLM 负责编写和维护。

## 快速开始

1. **安装 Obsidian**（obsidian.md），打开本仓库目录作为 vault。
2. 依提示安装社区插件 **Dataview**（强烈推荐）。
3. 打开 `wiki/index.md` 查看全局索引，打开图谱视图看知识结构。
4. 往 `raw/00-Inbox/` 丢一篇素材，然后用 LLM 对话工具说：

   > 请消化这篇素材，按照 `AGENTS.md` 的规范执行 ingest。

## 目录结构

```
llm-wiki/
├── AGENTS.md            # Schema：LLM 的行为规范（合同）
├── skills/llm-wiki/     # Agent Skill：工作流详细规则
├── raw/                 # 原始素材（人类写入，LLM 只读）
│   ├── 00-Inbox/        # 快速收集箱
│   ├── 01-Daily/        # 每日笔记
│   ├── 10-Work/         # 工作相关
│   ├── 20-Tech/         # 技术知识
│   ├── 30-Life/         # 生活日常
│   ├── 80-Templates/    # 笔记模板
│   ├── 90-Attachments/  # 图片、PDF 等附件
│   └── 95-Archive/      # 归档区
├── wiki/                # LLM 编译产物（LLM 读写，人类只浏览）
│   ├── index.md         # 全局索引
│   ├── log.md           # 操作日志
│   ├── entities/        # 实体页
│   ├── concepts/        # 概念页
│   ├── summaries/       # 素材摘要页
│   ├── comparisons/     # 对比分析页
│   ├── synthesis/       # 综合分析页
│   └── logs/            # 归档的历史日志
└── output/              # 成品输出（LLM 生成，人类审核）
    ├── posts/           # 博客文章
    ├── reports/         # 研究报告
    ├── slides/          # 演示文稿（Marp）
    ├── tutorials/       # 教程指南
    └── newsletters/     # 知识简报
```

**铁律：`raw/` 只读，`wiki/` 读写，绝不修改原始素材。**

## 工作流指令

| 指令 | 含义 |
|---|---|
| `ingest <素材>` | 消化一篇素材（摘要→实体/概念→双链→更新 index/log） |
| `query <问题>` | 基于 wiki 已编译的知识回答 |
| `lint` | 健康检查（矛盾/孤立页/缺失引用/过时声明） |
| `publish <主题> 为 <类型>` | 生成成品到 `output/` |
| `refresh <主题>` | 联网重校验过时内容 |

## 页面规范

每个 wiki 页面用 YAML frontmatter 声明 `title / aliases / tags / category / created / updated / sources / description`，详情见 `AGENTS.md`。

## 规模化

- **< 300 页**：单文件 index.md 够用
- **300-1000 页**：index 拆分为分类子索引
- **1000+ 页**：引入语义搜索，index 降级为辅助导航，log 按年归档

## 关键心态

不需要一次消化所有存量笔记。从今天开始，每次碰到新素材就 ingest 一篇，wiki 会自然生长，知识在持续复利。开始养你的知识花园吧。