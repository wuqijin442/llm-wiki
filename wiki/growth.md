# 🌱 生长看板

> **自生长 = 每次输入都在库里留下可复用的结构化变化。** 本页把"生长"变成可看的数字。
> 全部查询由 **Dataview** 插件渲染（设置 → 社区插件 → 开启 Dataview）。若显示为代码块，说明插件未启用。
> 只看不写：本页由 LLM 在每次 ingest / lint 后核对，人类随时打开巡检。

## 怎么读这一页

| 指标 | 健康状态 | 不健康说明什么 | 对应动作 |
| --- | --- | --- | --- |
| 未消化素材 | 0 | 有素材进了 `raw/` 却没进 `wiki/`，知识没有沉淀 | 执行 `ingest`（批量） |
| 零出链页面 | 0 | 页面成了孤岛，没有被接入知识网络 | 补 `[[双链]]` |
| 缺字段页面 | 0 | 索引与检索能力受损 | 补 `description` / `status` |
| 超 90 天未更新 | 0 | 内容可能已过时 | 执行 `refresh` 或标 `stale` |
| 低置信度页面 | 越少越好 | 存在未裁决的分歧或证据不足 | 人工裁决或补来源 |

> 任一项 > 0，就安排一次 `ingest` 或 `lint`。指标只增不减不是目标——**熵减（合并、纠错、补全）同样算生长。**

---

## 1. 页面总量与分类分布

```dataview
TABLE length(rows) AS 页数, rows.file.link AS 页面
FROM "wiki"
WHERE contains(file.folder, "wiki/") AND !contains(file.folder, "logs")
GROUP BY category
SORT length(rows) DESC
```

## 2. 按创建日期看增长

```dataview
TABLE length(rows) AS 当日新增, rows.file.link AS 页面
FROM "wiki"
WHERE contains(file.folder, "wiki/") AND !contains(file.folder, "logs")
GROUP BY created
SORT created DESC
```

## 3. 最近更新 Top 10

```dataview
TABLE category AS 分类, status AS 状态, updated AS 更新日期
FROM "wiki"
WHERE contains(file.folder, "wiki/") AND !contains(file.folder, "logs")
SORT updated DESC
LIMIT 10
```

---

## 4. 未消化素材（待 ingest）

`raw/` 里还没被任何 wiki 页 `sources` 引用的素材：

```dataview
TABLE file.folder AS 目录, file.mtime AS 修改时间
FROM "raw"
WHERE !contains(file.folder, "80-Templates") AND file.name != "README" AND length(file.inlinks) = 0
SORT file.mtime DESC
```

> **自查**：若这里列出了已消化过的素材，说明它的 `sources` 双链路径写错了（Obsidian 没解析到）。去对应 wiki 页修正 `sources` 为真实路径，如 `[[raw/20-Tech/文件名]]`。

## 5. 零出链页面（疑似孤岛）

```dataview
TABLE category AS 分类, updated AS 更新日期
FROM "wiki"
WHERE contains(file.folder, "wiki/") AND !contains(file.folder, "logs") AND length(file.outlinks) = 0
SORT updated ASC
```

> 注：本页按 **出链** 判定孤岛——`index.md` 的入链是"被登记"，不代表页面之间真的联通。

## 6. 字段缺失（影响索引与检索）

```dataview
TABLE category AS 分类, status AS 状态, description AS 摘要
FROM "wiki"
WHERE contains(file.folder, "wiki/") AND !contains(file.folder, "logs") AND (!description OR description = "" OR !status)
SORT updated ASC
```

## 7. 超 90 天未更新（stale 候选）

```dataview
TABLE category AS 分类, updated AS 最后更新
FROM "wiki"
WHERE contains(file.folder, "wiki/") AND !contains(file.folder, "logs") AND updated AND date(today) - updated > dur(90 days)
SORT updated ASC
```

## 8. 低置信度页面（存在未裁决分歧）

```dataview
TABLE category AS 分类, confidence AS 置信度, scope AS 适用范围, updated AS 更新
FROM "wiki"
WHERE contains(file.folder, "wiki/") AND !contains(file.folder, "logs") AND confidence = "低"
SORT updated DESC
```

---

## 9. 生长复盘（人工填写，每次 lint 后更新）

> **变动分解（AGENTS.md §4）**：`新增页`=added，`补充页`=revised，`合并/纠错`=merged/fixed。
> **churn** =（补充+合并/纠错）/（新增+补充+合并/纠错）——策展占比；churn≈0 且只增不修 → 安排熵减 lint。
> **travel** = 各期（新增+补充+合并/纠错）之和，跨期比较用它，不用端点页数。

| 日期 | 新增页 | 补充页 | 合并/纠错 | churn | 未消化素材 | 结论 |
| --- | --- | --- | --- | --- | --- | --- |
| 2026-09-12 | — | — | — | — | 0 | 完成 v2 自生长机制搭建 |
| 2026-09-12 | 1 | 22 | 1 | 0.96 | 0 | 24 页 status 回填完成（20 seed / 4 growing）；装 Dataview 0.5.70；建 2 条自动化（每日入库 / 每周 lint） |
| 2026-09-12 | 3 | 1 | 0 | 0.25 | 0 | 自动化 ingest：消化 VPN 客户端素材，新建摘要 1 + 概念 2；[[个人AI工程方法论]] seed→growing（sources 2） |
| 2026-09-14 | 3 | 3 | 0 | 0.50 | 0 | 消化掘金「双环知识飞轮」：新建摘要 1 + 概念 2；[[LLM-Wiki-vs-RAG]]、[[双链网络]] seed→growing；每日素材采集源扩展为 CSDN / GitHub / Gitee / arXiv |
| 2026-09-14 | 0 | 0 | 0 | — | 0 | 只读 lint（每周一 09:00 自动化）：四项指标全绿（未消化素材/零出链/缺字段/超90天未更新均0）；无矛盾、无断链、无缺失概念页；发现 index.md 素材总数漂移(记16→实为15)、孤立页 ComfyUI(仅被 index 登记、0 内容入链)；23 seed/7 growing 符合单来源规则 |
| 2026-09-14 | 1 | 1 | 0 | 0.50 | 0 | 每日素材采集(第0步)产出 raw/00-Inbox/每日AI素材-2026-09-14.md，消化为摘要页 1 + [[Agent]] 趋势补充 1；素材 16→17、页 30→31 |
| 2026-09-15 | 1 | 1 | 0 | 0.50 | 0 | 每日素材采集(第0步)产出 raw/00-Inbox/每日AI素材-2026-09-15.md，消化为摘要页 1 + [[Agent]] 趋势补充 1（RSI/agent 工具化）；素材 17→18、页 31→32 |
| 2026-09-16 | 4 | 3 | 0 | 0.43 | 0 | Proteus 仓库精读入库：新建摘要 1 + 实体 1 + 概念 2；[[双环知识飞轮]] seed→growing；素材 18→19、页 32→36；AGENTS.md → v2.1（O-P-A-R / 变动分解 / disposition 协议 / 比较可靠性） |
| 2026-09-16 | 1 | 2 | 0 | 0.67 | 0 | 每日素材采集(第0步)产出 raw/00-Inbox/每日AI素材-2026-09-16.md，消化为摘要页 1 + [[Agent]] 趋势子节补充 + [[Harness自进化]] 外部印证出链；素材 19→20、页 36→37 |

> **累计 travel（至 2026-09-16）** = 24+4+6+2+2+7+3 = **48** 单位变动（不含机制搭建行）。
> 复盘只记"变化"，不记流水。参照 `wiki/log.md` 的详细记录填写。
