# 🔄 生命周期台账

> 页面可以过时、可以被取代，但**知识不可被删除**。
> 本页记录每个 wiki 页面处在哪个阶段、何时需要复核。
> 规则见 `AGENTS.md` §6；生长指标见 `wiki/growth.md`。

## 状态定义

```
seed ──→ growing ──→ mature ──→ stale ──→ superseded
（新建）  （迭代中）  （稳定）   （过时）   （被取代·留档）
```

| 状态 | 判定 | 处置 | 复核节奏 |
| --- | --- | --- | --- |
| `seed` | 仅 1 个来源，或存在空章节 | 下次 ingest 优先补充 | 下次 ingest |
| `growing` | 2+ 来源，仍在迭代 | 正常维护 | 月度 |
| `mature` | 多来源一致，无「待核实」 | 抽查即可 | 季度 |
| `stale` | `updated` 超 90 天，或来源明显过时 | 执行 `refresh` 联网重校验 | 立即 |
| `superseded` | 已被新页面取代 | **保留存档不删除**，加 `superseded_by` 指向新页 | 不再维护 |

> 只有 `superseded` 和 `stale` 需要人工介入判断；其余交给 Agent 在增量维护时自动流转。

---

## 1. 状态分布

```dataview
TABLE length(rows) AS 页数, rows.file.link AS 页面
FROM "wiki"
WHERE contains(file.folder, "wiki/") AND !contains(file.folder, "logs")
GROUP BY status
SORT length(rows) DESC
```

## 2. seed 页（待补充，优先处理）

```dataview
TABLE category AS 分类, sources AS 来源, updated AS 创建/更新
FROM "wiki"
WHERE contains(file.folder, "wiki/") AND !contains(file.folder, "logs") AND status = "seed"
SORT updated ASC
```

## 3. mature 页（已稳定）

```dataview
TABLE category AS 分类, updated AS 最后更新
FROM "wiki"
WHERE contains(file.folder, "wiki/") AND !contains(file.folder, "logs") AND status = "mature"
SORT updated DESC
```

## 4. stale 页（需 refresh）

```dataview
TABLE category AS 分类, updated AS 最后更新
FROM "wiki"
WHERE contains(file.folder, "wiki/") AND !contains(file.folder, "logs") AND (status = "stale" OR (updated AND date(today) - updated > dur(90 days)))
SORT updated ASC
```

## 5. superseded 存档（被取代，仅留痕）

```dataview
TABLE superseded_by AS 被谁取代, updated AS 取代时间
FROM "wiki"
WHERE contains(file.folder, "wiki/") AND !contains(file.folder, "logs") AND status = "superseded"
SORT updated DESC
```

---

## 6. 复核台账（人工填写）

| 复核日期 | 复核人 | 范围 | 结论 | 处置 |
| --- | --- | --- | --- | --- |
| 2026-09-12 | — | 全库（22 页，首次建立台账） | 均为 `growing` / 待回填 `status` | 增量维护时逐个补齐 `status` |
| 2026-09-12 | — | 全库（24 页，`status` 回填完成） | 20 `seed` / 4 `growing`；无 `stale`、无 `superseded` | `seed` 页在下轮 ingest 优先补充；`seed` 偏多属正常（多数页面仅 1 个来源） |
| 2026-09-12 | — | 增量（新增 3 页 + 1 页状态流转） | 新 3 页均 `seed`；[[个人AI工程方法论]] → `growing`（sources 2）；全库 22 `seed` / 5 `growing` | 新 `seed` 页待后续 ingest 补第二来源；ITMS-91054 对应关系待联网 `refresh` 核验 |
| 2026-09-14 | — | 增量（新增 3 页 + 2 页状态流转） | 新 3 页均 `seed`（[[双环知识飞轮]] / [[ICAP学习框架]] / [[双环知识飞轮-摘要]]）；[[LLM-Wiki-vs-RAG]]、[[双链网络]] → `growing`（sources 2）；全库 23 `seed` / 7 `growing` | ICAP 原始论文待联网 `refresh` 核验；「回流带」落地规则（会话来源 / 提炼标准 / 去重）未定义，暂记于 [[双环知识飞轮-摘要]] §待核实 |
| 2026-09-14 | — | 全库只读 lint（30 内容页 + 5 元页面 + 15 知识素材） | 字段全齐(description/status/category/sources 30/30)；无矛盾、无断链、无缺失概念页、无未消化素材、无 stale(均 2026-09-12/14 更新)；孤立页 ComfyUI(0 内容入链)；index 素材总数记 16 实为 15 | 两项为机械修复项，本轮仅追加台账未改内容页；index 计数与 ComfyUI 入链留待下次 ingest 补；23 `seed` 页待后续补第二来源升 `growing` |
| 2026-09-15 | — | 增量（新增 1 页 + 1 页补充） | 新 1 页 `seed`（[[每日AI素材-2026-09-15-摘要]]）；[[Agent]] 补趋势观察（sources 4，保持 `growing`）；全库 25 `seed` / 7 `growing` | 新 `seed` 页待下次 ingest 补第二来源；RSIAgent 超 GPT-6 Astra 数值待联网 `refresh` 核验（见摘要 §待核实） |

## 7. 日志归档规则

- `wiki/log.md` 保留最近 30 天记录，更早的按年归档到 `wiki/logs/YYYY.md`（例：`wiki/logs/2026.md`）。
- 归档只搬移，不改写；归档后在本页记一条复核台账。
