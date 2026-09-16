# LLM Wiki Schema

> 本文件是 LLM 与人类协作维护本知识库的行为规范（“合同”）。
> LLM 在 ingest / query / lint / publish / refresh 等任何操作前，**必须先读取本文件**。
> 人类也可根据实际体验逐步修改本文件，使其持续演进。
>
> **心智模型（Karpathy）**：Obsidian 是“IDE”，LLM 是“程序员”，`wiki/` 是“代码库”，`raw/` 是不可改写的“证据”，本文件是“编码规范”。
>
> 版本：v2.1 ｜ 更新：2026-09-16 ｜ 相对 v2 变更：Ingest/Lint 协议化为 observe→propose→act→reflect；§4 增补变动分解（churn / travel）；新增 §8 可移除 disposition 实验协议；Lint 增补「比较前可靠性」门槛。方法论参考 Proteus 精读（见 `wiki/concepts/自进化测量标尺`）。

---

## 1. 目录结构与权限

知识库采用四层架构，各层职责清晰，权限严格。

| 分区        | 用途                           | 权限规则                                       |
| ----------- | ------------------------------ | ---------------------------------------------- |
| `raw/`      | 原始素材（人类写入）           | **只读**。LLM 绝不修改、删除、重命名任何文件   |
| `wiki/`     | 编译产物（LLM 读写）           | 读写。LLM 负责创建和维护所有页面，人类只浏览   |
| `output/`   | 成品输出（协作区）             | 读写。LLM 生成草稿，人类审核定稿               |
| `skills/`   | Agent Skill 工作流详细规则     | 只读。按需加载                                 |

> 对应 Karpathy 的三层心智：`raw/` 是 Raw（证据层），`wiki/` 是 Wiki（理解层），`AGENTS.md` + `skills/` 是 Schema（规则层）；`output/` 是规则层的下游产物区。

### raw/ 子目录

| 目录               | 用途                             |
| ------------------ | -------------------------------- |
| `raw/00-Inbox/`    | 快速收集箱（临时笔记、待分类素材） |
| `raw/articles/`    | 网页剪藏（Obsidian Web Clipper 落地目录） |
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
| `wiki/仪表盘.md`   | 人读看板（Dataview 概览）  |
| `wiki/growth.md`   | 生长看板（增长 / 熵减指标）|
| `wiki/lifecycle.md`| 生命周期台账（成熟度与复核）|
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
status: growing      # 可选：seed | growing | mature | stale | superseded
confidence: 中        # 可选：高 | 中 | 低
scope: 适用范围       # 可选：版本 / 平台 / 场景等边界说明
---
```

规范要求：

- `category`：必填，取值必须严格落在这五类：`entities`、`concepts`、`summaries`、`comparisons`、`synthesis`
- `created` / `updated`：使用 `YYYY-MM-DD` 格式
- `sources`：素材来源必须用 `[[双链]]` 指向 raw 层
- `description`：不超过 100 字的一句话摘要，用于 Dataview 查询展示
- `status` / `confidence` / `scope`：可选字段，**新建页面必须填写 `status`**；存量页面在增量维护时顺手补齐即可，不必批量回填
- `supersedes` / `superseded_by`：可选，用于记录观点被取代的链路（见 §5）
- 页面内通过 `[[双链]]` 与其他页面交叉引用
- **元页面豁免**：`index.md`、`log.md`、`仪表盘.md`、`growth.md`、`lifecycle.md` 是导航与台账，不是知识页，不受上述 frontmatter 约束

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

## 4. 自生长机制

**自生长 = 每次输入都在库里留下可复用的结构化变化。**

判定标准：一次 ingest 结束后，`wiki/` 必须出现 diff（新增页 / 补充章节 / 新增双链 / 新增「待核实」条目）。若只有 `raw/` 多了一个文件而 `wiki/` 毫无变化，**视为本次 ingest 失败**。

每次 ingest 必做四个动作：

| 动作 | 内容 | 落点 |
| --- | --- | --- |
| ① 差异检测 | 列出 `raw/` 中尚未被任何 wiki 页 `sources` 引用的素材，确认本次待消化集合 | `wiki/growth.md` →「未消化素材」 |
| ② 增量归并 | **优先补充已有页面**；只有无法归入任何现有页的概念/实体才新建页 | `wiki/entities/`、`wiki/concepts/` |
| ③ 交叉链接 | 新页至少 2 条出链、至少 1 条入链，并登记进全局索引 | `wiki/index.md` |
| ④ 留痕 | 追加日志；刷新页面 `updated` 与 `status`；缺口与冲突写入「待核实」 | `wiki/log.md` |

**熵减也算生长。** 合并重复页、修正错误结论、补全缺失字段、清掉断链、把 `stale` 页刷新——与新增页面同等重要。指标只增不减不是目标。

**变动分解（区分堆积与策展）。** 每次 ingest 在 `growth.md` 复盘表按单位记录三类变动，而不是只看页数：

| 记号 | 含义 |
| --- | --- |
| `added` | 新建页数 |
| `revised` | 补充/改写既有页数 |
| `merged/fixed` | 合并、纠错、删除性熵减数 |

- **churn** = (revised + merged/fixed) / (added + revised + merged/fixed)——策展在变动中的占比。持续 churn≈0 的「只堆不炼」应触发一次 lint 熵减；churn 高不是坏事，是另一种生长质量。
- **travel（路径长度）**：复盘表各期 (added+revised+merged/fixed) 的累计和。跨期比较用 travel，不用端点页数——页数增长早期即饱和，路径长度才保有分辨率。

**生长检查点**（建议按周执行）：打开 `wiki/growth.md` 看四项指标——未消化素材数、孤立页数、缺 `description` 页数、超 90 天未更新页数。任一项 > 0 即安排一次 `ingest` 或 `lint`。跨期比较生长前，先满足 §7 Lint 的「比较前可靠性」门槛。

**增量优先于重写。** 任何时候都不需要"重建整个知识库"；只处理新增部分与受影响页面。

---

## 5. 冲突与不确定性处理

遇到不同说法时**禁止悄悄抹平**——这是 LLM Wiki 区别于 RAG 的关键。原则：保留来源、时间与适用范围。

| 情况 | 处理方式 |
| --- | --- |
| 同一事实的不同表述 | 并列保留两种说法，各自标注 `sources` 与日期，不合并成一句"综合结论" |
| 新旧结论冲突 | 按时间记录演进；旧结论标 `status: superseded`，用 `superseded_by` 指向新页面，正文说明被取代的原因 |
| 适用范围不同 | 正文注明适用边界（版本 / 平台 / 场景），并填写 `scope` 字段 |
| 证据不足 | 在页面「待核实」小节列出问题，**不编造结论**，并标 `confidence: 低` |
| 完全无法判定 | 保留争议现状，在 `wiki/growth.md` 的「低置信度页面」中暴露，交由人类裁决 |

页面内统一使用小节标题 `## 待核实`（无内容可省略）。这是给未来自己的备忘录：不要用一段看似完整的总结，把真实存在的分歧掩盖掉。

---

## 6. 页面生命周期

状态流转：`seed` → `growing` → `mature` →（`stale` → `superseded`）

| 状态 | 判定 | 处置 |
| --- | --- | --- |
| `seed` | 仅 1 个来源，或存在空章节 | 下次 ingest 优先补充 |
| `growing` | 2+ 来源，仍在迭代 | 正常维护 |
| `mature` | 多来源一致，无「待核实」 | 季度抽查 |
| `stale` | `updated` 超 90 天，或来源明显过时 | 执行 `refresh` 联网重校验 |
| `superseded` | 已被新页面取代 | **保留存档不删除**，加 `superseded_by` |

生命周期台账见 `wiki/lifecycle.md`；页面可被取代，知识不可被删除。

---

## 7. 工作流程定义

### Ingest（摄入：消化一篇素材）

协议：`observe → propose → act → reflect`（四相位）。目的是把「观察—提案—行动—反射」显性化：**没有 propose 清单不得直接 act；没有 reflect 留痕视为未完成**。上下文新鲜时从上一相位结论续起，不靠对话记忆。

**observe（观察）**

0. **差异检测**：列出 `raw/` 中未被任何 wiki 页引用的素材，明确本次消化范围（单篇 or 批量）
1. 读取素材全文，提取关键信息；顺带扫一眼 `growth.md` / `lifecycle.md` 相关指标（未消化、seed 堆积、stale）

**propose（提案）**

2. 向用户汇报关键发现与**拟建页面清单**（新建哪些、补充哪些、如何双链、预计冲突点），确认重点

**act（行动）**

3. 创建摘要页到 `wiki/summaries/`
4. **增量归并**：先检索已有实体页/概念页——命中则补充内容与 `sources`，仅新建确实缺失的页面
5. 在页面之间建立 `[[双链]]` 交叉引用（新页 ≥2 出链、≥1 入链）
6. 遇到说法冲突，按 §5 记录来源 / 时间 / 适用范围，不抹平

**reflect（反射）**

7. 更新 `wiki/index.md` 与 `wiki/log.md`；刷新相关页面 `updated` 与 `status`
8. 在 `growth.md` 复盘表记变动分解（`added` / `revised` / `merged|fixed` 与 churn）；**记录本 episode 的意外与未解问题**（预期外的冲突、未消化的疑点、下轮建议）——不只记成功流水

### Query（查询：基于已编译知识回答）

1. 读取 `wiki/index.md` 定位相关页面
2. 阅读相关页面内容
3. 综合各页面合成答案
4. 在回答中提供 `[[引用]]`，同时标注 wiki 页面与 raw 原文路径
5. 单独列出资料中的不同观点、信息缺口与待核实项
6. 询问用户是否将优质答案存档（对比分析存 `comparisons/`，综合洞察存 `synthesis/`）；回答中产生的新概念、新关联经确认后回写为 wiki 页面（自生长闭环）

### Lint（健康检查：定期维护）

检查以下项目（结论写入 `wiki/growth.md` 与 `wiki/log.md`）：

- 矛盾检测：不同页面关于同一主题的描述是否冲突
- 孤立页面：无任何 `[[入链]]` 的页面
- 缺失概念页：被引用但尚未创建的概念
- 过时声明：明显过时的内容
- 交叉引用完整性：断链、`sources` 指向不存在的 raw 文件
- 未消化素材：`raw/` 中存在但无任何 wiki 页引用的素材
- 字段完整性：缺 `description` / 缺 `status` / `sources` 为空的页面
- 生命周期：`updated` 超 90 天的 `stale` 页、长期停留在 `seed` 的页
- **比较前可靠性**：若本次目的包含「与上期/上月比较生长」，先抽查口径自复现——对同一固定输入集（如 3 篇指定素材或 5 页指定页面）重跑检查项，结论不一致则**本次比较作废**，先修口径再比；不一致本身记入 `wiki/log.md`
- 生成修复建议，用户确认后修复；修复后记录到 `wiki/log.md`

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
4. 重新 ingest 更新 wiki，并将旧结论按 §5 标记为 `superseded`
5. 更新 `wiki/lifecycle.md` 的复核时间与 `wiki/log.md`

---

## 8. 可移除 disposition 实验协议

借鉴 Proteus 的 disposition / 结晶测试（见 `wiki/concepts/自进化测量标尺`）：对**维护习惯的注入**（新的 PROMPT 要求、附加 checklist、每周必做清单）不要直接永久写入 Schema，先以**可移除、单一、有期限**的形式登记实验，验证其是否被真正内化。

1. **登记**：实验开始前在 `wiki/log.md` 记一条「disposition 实验」——注入内容（**单一差异**，一次只测一个习惯）、载体（PROMPTS 片段 / 临时 checklist / 会话提示）、期限（建议 3–5 次同类操作）、**预期可观测行为**（例如「复盘表必填 churn」）。
2. **执行**：期限内每次相关操作都附带该注入；在 log 简短记录是否遵守（遵守/遗漏/部分）。
3. **移除**：到期撤除注入，**不留残句**（从 PROMPTS/checklist 中物理删除该段）。
4. **结晶判读**：随后 2–3 次同类操作中，若预期行为仍出现 → 已结晶，可提案升格为正式 Schema 条款（走人工确认）；若消失 → 未结晶，注入无效或成本过高，**不升格**。
5. **约束**：实验数据（遵守率）只服务人与 Schema 演进，**不写入被维护页面的正文**，防止污染知识本身；label 用不透明 id，但 log 里可写清实验目的（人类是裁决层）。

对应关系：单一可移除扰动 + 移除后读回身份 = 结晶测试。本库「被测物」是**维护流程与 Schema**，不是模型权重；「结晶」= 习惯在撤除提示后仍被执行。
