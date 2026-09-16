# 🧠 llmWiki — 自生长个人知识库

Obsidian + LLM 自增长知识库（LLM Wiki 模式）。基于 Karpathy 公开的 **LLM wiki** 构建方法与 [《用 WorkBuddy / Codex + Obsidian 搭建自生长的个人知识库实战》](https://zhuanlan.zhihu.com/p/2069823967242740050) 落地。

> **比喻（Karpathy）**：Obsidian 是"IDE"，LLM 是"程序员"，`wiki/` 是"代码库"，`raw/` 是不可改写的"证据"。
> 你在 Obsidian 里浏览和审阅，LLM 负责编写和维护。

## 为什么是这一套

| 方案 | 问题 |
| --- | --- |
| 传统 RAG / 文件上传问答 | 每次提问都从碎片里**重新拼答案**，答完就散。同样的问题问一百遍，拼一百遍，知识没有任何积累 |
| **LLM Wiki** | 每加入一份资料，Agent 先看现有页面 → 补充已有内容 → 新概念单独建页 → 遇分歧保留来源、时间与适用范围。**知识持续复利** |

"自生长"的关键不是自动化，而是：**AI 处理完资料后，知识库必须留下变化**——新增一个概念、补上一条关联，或暴露一个暂时没有答案的问题。一次次变化累积起来，才长成自己的知识体系。

## 三层架构

| 层 | 目录 | 职责 |
| --- | --- | --- |
| **Raw（证据层）** | `raw/` | 保存文章、论文、聊天记录等原始素材。人类写入，**LLM 只读** |
| **Wiki（理解层）** | `wiki/` | 沉淀 AI 整理出的实体、概念、摘要、对比、综合。**LLM 读写**，人类只浏览 |
| **Schema（规则层）** | `AGENTS.md` + `skills/` | 规定如何归档、更新、引用，以及怎么处理冲突 |
| Output（产物区） | `output/` | 草稿与成品，LLM 生成、人类审核 |

**铁律：`raw/` 只读，`wiki/` 读写，绝不修改原始素材。**

## 目录结构

```
llmWiki/
├── AGENTS.md            # Schema：LLM 的行为规范（合同）v2
├── PROMPTS.md           # 提示词手册（复制即用）
├── README.md            # 本文件
├── skills/llm-wiki/     # Agent Skill：工作流详细规则
├── raw/                 # 原始素材（人类写入，LLM 只读）
│   ├── 00-Inbox/        # 快速收集箱
│   ├── articles/        # 网页剪藏（Web Clipper 落地）
│   ├── 01-Daily/        # 每日笔记
│   ├── 10-Work/         # 工作相关
│   ├── 20-Tech/         # 技术知识
│   ├── 30-Life/         # 生活日常
│   ├── 80-Templates/    # 笔记模板
│   ├── 90-Attachments/  # 图片、PDF 等附件
│   └── 95-Archive/      # 归档区
├── wiki/                # LLM 编译产物（LLM 读写，人类只浏览）
│   ├── index.md         # 全局索引（内容导向）
│   ├── log.md           # 操作日志（时间导向）
│   ├── 仪表盘.md        # 人读看板
│   ├── growth.md        # 🌱 生长看板（增长 / 熵减指标）
│   ├── lifecycle.md     # 🔄 生命周期台账（成熟度与复核）
│   ├── entities/        # 实体页
│   ├── concepts/        # 概念页
│   ├── summaries/       # 素材摘要页
│   ├── comparisons/     # 对比分析页
│   ├── synthesis/       # 综合分析页
│   └── logs/            # 按年归档的历史日志
└── output/              # 成品输出（LLM 生成，人类审核）
    ├── posts/  reports/  slides/  tutorials/  newsletters/
```

## 快速开始

1. **装 Obsidian**（obsidian.md），把本目录作为 Vault 打开。
2. **开启 Dataview 插件**：本仓库已预装官方 **Dataview 0.5.70**（位于 `.obsidian/plugins/dataview/`）并在 `.obsidian/community-plugins.json` 中登记启用。**重启 Obsidian 后生效**；若设置里仍显示受限模式，手动把「第三方插件」开关打开即可。这是 `仪表盘` / `growth` / `lifecycle` 三个看板的渲染前提。
3. 装浏览器扩展 **Obsidian Web Clipper**，默认目录设为 `raw/articles`（详见 `raw/articles/README.md`）。
4. 剪藏一篇文章，然后在 WorkBuddy / Codex 里说：

   ```text
   请读取 raw/ 中新增的素材，并严格按照 AGENTS.md 的规则增量 ingest。
   ```

5. 打开 `wiki/index.md` 看索引，打开 `wiki/growth.md` 看生长指标，打开图谱视图看知识结构。

## 工作流指令

| 指令 | 含义 |
| --- | --- |
| `ingest <素材>` | 消化素材（O-P-A-R：观察 → 提案 → 行动 → 反射；含变动分解与 churn） |
| `query <问题>` | 基于 wiki 已编译的知识回答，附引用与 raw 原文路径 |
| `lint` | 健康检查（矛盾 / 孤岛 / 断链 / 缺字段 / 过时 / 未消化素材；比较前可靠性） |
| `publish <主题> 为 <类型>` | 生成成品到 `output/`（双链转标准链接） |
| `refresh <主题>` | 联网重校验过时内容，旧结论标 `superseded` |
| `生长复盘` | 核对生长指标、churn/travel 与生命周期状态 |
| `disposition 实验` | 可移除习惯注入实验（AGENTS.md §8），到期结晶判读 |

完整提示词见 **`PROMPTS.md`**。

## 自生长闭环

```
素材进入 raw/  →  Agent 按 AGENTS.md 检索现有 wiki
                      ↓
      补充已有页面 / 新建缺失页面 / 建立双链 / 记录冲突
                      ↓
      index.md + log.md 留痕 · growth.md 指标变化
                      ↓
      query 产生的优质答案回写 comparisons/ 或 synthesis/
                      ↓
      lint 熵减：合并、纠错、补全、刷新 stale
                      ↓
                 知识库继续生长
```

## 三种接入方式

| 方式 | 说明 | 适合 |
| --- | --- | --- |
| **① 直接搭建法（本库现状）** | 靠 `AGENTS.md` + `PROMPTS.md` 提示词驱动 Agent，规则完全可自定义 | 想深度掌控 Schema、愿意持续调规则 |
| ② Skill 封装法 | 本库自带 `skills/llm-wiki/SKILL.md`，把 ingest / query / lint 固化成可复用 Skill，不必每次重写提示词 | 想少写提示词、稳定复用 |
| ③ Obsidian 插件法 | 在 Obsidian 内装知识库类插件（如 WeSight 知识大脑），把入库/检索搬进笔记界面，由后台 Agent 维护结构 | 想全程不离 Obsidian、看可视化过程 |

三种方式可叠加：先用 ① 把结构跑通，再用 ② 固化流程，需要界面化时再上 ③。

## 规模化建议

- **< 300 页**：单文件 `index.md` 够用
- **300-1000 页**：把 index 拆为分类子索引（`index-entities.md` 等）
- **1000+ 页**：引入全文/语义搜索，index 降级为辅助导航，`log.md` 按年归档到 `wiki/logs/`

## 关键心态

自生长**不等于完全自动化**。哪些资料值得保留、规则如何设定、关键结论能否成立，仍然需要人的判断；AI 更适合承担重复、耗时且结构化的维护工作。

不需要一开始就搭出完美系统：**先建好三层目录 → 放入一篇真实资料 → 让 Agent 完成第一次增量更新 → 再根据实际使用不断调整 Schema。**
