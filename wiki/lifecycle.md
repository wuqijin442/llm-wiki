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
| 2026-09-16 | — | 增量（新增 4 页 + 3 页补充 + 1 页状态流转） | 新 4 页均 `seed`（[[Proteus-自进化harness框架-摘要]] / [[Proteus]] / [[Harness自进化]] / [[自进化测量标尺]]）；[[Agent]]、[[双环知识飞轮]]、[[AI智能体工程方法论]] 补 Proteus 关联内容；[[双环知识飞轮]] → `growing`（sources 2）；全库 28 `seed` / 8 `growing` | 新 `seed` 页待后续 ingest 补第二来源；R=1.63/0.93 与 v0.3.0 清单待对照论文或上游更新 `refresh`；机制升级（AGENTS.md/growth）方案待用户确认后另案落地 |
| 2026-09-16 | — | 增量（每日素材采集 + 1 页补充） | 新 1 页 `seed`（[[每日AI素材-2026-09-16-摘要]]）；[[Agent]] 补「每日素材信号（2026-09-16）」H3 子节、[[Harness自进化]] 补外部印证出链；全库 29 `seed` / 8 `growing` | 新 `seed` 页待下次 ingest 补第二来源；GitHub/Gitee/arXiv 数值为页面声称值未独立核验（见摘要 §待核实）；Agent.md 已有 09-16 Proteus 趋势节，本次用 H3 子节规避 H2 重复 |
| 2026-09-18 | — | 增量（每日素材采集 + 2 页补充） | 新 1 页 `seed`（[[每日AI素材-2026-09-18-摘要]]）；[[Agent]] 补「每日素材信号（2026-09-18）」H3 子节、[[Harness自进化]] 补外部印证出链；全库 30 `seed` / 8 `growing` | 新 `seed` 页待下次 ingest 补第二来源；09-17 空缺未回填（当日口径不可复现）；GitHub/Gitee/arXiv/媒体日报/新浪数值多数为声称值未对照原文（见摘要 §待核实）；Agent.md 已沿用 H3 子节规避重复 H2 |
| 2026-09-19 | — | 增量（每日素材采集 + 2 页补充） | 新 1 页 `seed`（[[每日AI素材-2026-09-19-摘要]]）；[[Agent]] 补「每日素材信号（2026-09-19）」H3 子节、[[Harness自进化]] 补外部印证出链；全库 31 `seed` / 8 `growing` | 新 `seed` 页待下次 ingest 补第二来源；CSDN 接口字段缺失致该渠道本日降级；ECC Star 数、OpenWiki 案例细节、Atria Dawn 数值等均待核（见摘要 §待核实） |
| 2026-09-19 晚 | — | 增量（每日素材采集晚间第2批 + 2 页补充） | 新 1 页 `seed`（[[每日AI素材-2026-09-19-晚-摘要]]）；[[Agent]] 补「每日素材信号（2026-09-19-晚）」H3 子节、[[Harness自进化]] 补外部印证 2026-09-19-晚（HarnessTax + arXiv 2609.20474）；全库 32 `seed` / 8 `growing` | 新 `seed` 页待下次 ingest 补第二来源；GitHub/Gitee/arXiv 数值为页面声称值未独立核验；HarnessTax 为 BYOBot 媒体转述未对照原文；CSDN 接口字段缺失致该渠道降级；同日第 2 批文件名带 `-晚` 后缀，索引/摘要已标注防歧义 |
| 2026-09-20 | — | 增量（每日素材采集 + 2 页补充，链接纪律升级） | 新 1 页 `seed`（[[每日AI素材-2026-09-20-摘要]]）；[[Agent]] 补「每日素材信号（2026-09-20）」H3 子节、[[Harness自进化]] 补外部印证 2026-09-20（2609.20804 / SoL-Pi / C2C）；全库 33 `seed` / 8 `growing` | 新 `seed` 页待下次 ingest 补第二来源；GitHub/Gitee/arXiv 数值为页面声称值未独立核验；AGI HUNT / 今日头条为媒体口径未对照原文；CSDN 接口字段缺失致该渠道降级；**本轮起每日素材条目强制附可点击链接**（写入 `skills/llm-wiki/SKILL.md`「采集链接纪律」） |
| 2026-09-20 晚 | — | 增量（每日素材采集晚间第2批 + 2 页补充） | 新 1 页 `seed`（[[每日AI素材-2026-09-20-晚-摘要]]）；[[Agent]] 补「每日素材信号（2026-09-20 晚）」H3 子节、[[Harness自进化]] 补外部印证 2026-09-20 晚（推理路由经济学 2609.15992 / REALM 记忆再巩固 2609.16055 / Agents-Flex 把 LLM Wiki 列为内置）；全库 34 `seed` / 8 `growing` | 新 `seed` 页待下次 ingest 补第二来源；GitHub/Gitee/arXiv 数值为页面声称值未独立核验；WebSearch 补充取自 09-16/09-17 简报未逐条对照原文；CSDN 接口本轮恢复（此前多日降级，数据连续性存疑）；同日第 2 批文件名带 `-晚` 后缀，索引/摘要已标注防歧义 |

| 2026-09-21 | — | 全库只读 lint（42 内容页 + 5 元页面 + 24 知识素材，排除 80-Templates 与 articles/README） | 字段全齐(description/status/sources 42/42)；sources 全部解析成功(0 断链)；未消化素材 0；无 stale(均 ≤2026-09-20)；矛盾检测 spot-check 无硬冲突；孤立页 3(ComfyUI / GitHub-AI周报2026-08-09 / Proteus-自进化harness框架-摘要)、零出链 7(Proteus / 自进化测量标尺 / 每日AI素材-2026-09-15·16·18·20-摘要 / Proteus-摘要)、Harness自进化 status=seed 但 sources=4(违反 09-12 source-count 规则)、index 素材总数记 25 实为 24(09-14 遗留 +1 漂移) | index 计数与 Harness自进化 status 为需人工裁决项；3 孤岛与 7 零出链页为机械修复项，本轮仅追加台账未改任何 wiki 内容页/raw/.obsidian；34 个 seed 页待后续 ingest 补第二来源升 growing |
| 2026-09-21 修复 | — | 全库修复（9 内容页 + index） | 应用 09-21 lint 三项修复：① index 素材总数 25→24（消 09-14 遗留 +1 漂移）；② 解包 5 页反引号死链 36 处，新增每日20 关联页与 2 条入链（本地优先AI创作→ComfyUI、Agent→GitHub-AI周报），孤岛 3→0、零出链 7→0（每日18 为脚本误报已排除）；③ Harness自进化 status seed→growing（sources=4 合规）；修复后经 lint 复核：孤立页 0 / 零出链 0 / 断链 0 / 字段完整 42/42 / status 与 sources 一致（33 seed / 9 growing）；未改 raw/.obsidian，未构建未提交 | 34→33 个 seed 页待后续 ingest 补第二来源升 growing |
| 2026-09-21 遗留 | — | 全库 33 seed 页 + skills/llm-wiki/SKILL.md | 33 seed 全部 status 与 sources 一致（均 1 来源）；raw 24/24 已消化无闲置素材；其中 9 素材拆多张角度页（正常策展非重复）；判定 seed 为 deferred-normal 非缺陷，不可机械加源（违反§5）；已将 lint 误报根因固化进 SKILL.md 防坑小节（反引号死链排除/正则不过度匹配/元页面合法目标/status-sources 一致性/一源多角勿合并） | 33 seed 待后续 ingest 补第二来源；SKILL.md 防坑已固化；工作树未提交改动待用户「提交」后落盘 |
| 2026-09-21 入库 | — | 增量（每日素材采集 + 3 页补充） | 新 1 页 `seed`（[[每日AI素材-2026-09-21-摘要]]）；[[Agent]] 补「每日素材信号（2026-09-21）」H3 子节、[[Harness自进化]] 补外部印证 2026-09-21（技能从代码库挖掘成共识 / harness 测量须绑定上下文窗口 / Skills over MCP 协议化）、[[RAG]] 补「知识即可执行：论文即 MCP Agent」（Paper2Agent）；全库 34 `seed` / 9 `growing` | 新 `seed` 页待下次 ingest 补第二来源；CSDN 接口连续降级 0 有效条目按纪律记「标题未取到」；GitHub/Gitee/arXiv 数值为页面声称值未独立核验；ZCode 外泄/SEP-2640/Paper2Agent 数字来自第三方转述待对照原文（见摘要 §待核实）；本批次连同 09-21 未提交 lint 修复一并纳入自动提交 |
| 2026-09-22 refresh | — | 联网核验（09-21 三处待核实 + 3 页 revised） | ZCode 外泄→多源已核实且事件已闭环（智谱开源 v3.14.0 + 信通院/绿盟审计确认 OSS 数据已删）；SEP-2640→经 modelcontextprotocol.io 官网确认 Final，补「SDK 仍 open、客户端不可用」caveat；Paper2Agent→Nature 一手 + AI Weekly 交叉确认 74/100、593/599、98.7% 等数字；新增 raw/00-Inbox/refresh-verify-2026-09-22.md 作第二来源；[[Agent]]/[[Harness自进化]]/[[RAG]] 三页 sources 各 +1、updated→2026-09-22 | 三处待核实已闭环（见摘要 §待核实进展 + refresh-verify 记录）；其余 4 处待核实（CodeMidas/GraphSkillEvo/DisCo 数值、harness 176 消融、GitHub/Gitee/arXiv 页面声称值、MiniCPM5-2B/FastContext）按范围纪律留待下轮；CSDN 纪律维持「标题未取到」 |

| 2026-09-22 入库 | — | 增量（每日素材采集 + 4 页补充 + 1 页状态流转） | 新 1 页 `seed`（[[每日AI素材-2026-09-22-摘要]]）；[[Agent]] 补「每日素材信号（2026-09-22）」H3 子节、[[Harness自进化]] 补外部印证 2026-09-22（Harness-Zero 蒸馏 + agent/harness 运行时霸榜 + Salesforce Enterprise AI Harness 商品化）、[[AI智能体分权治理]] 补「每日信号（2026-09-22）」(UN 面板/Meta Muse/共谋论文)、[[RAG]] 补「记忆成本前沿与 RSI 医疗化」(DolphinBench/语义熵路由/MedRSI)；[[AI智能体分权治理]] seed→growing（sources 2）；全库 34 `seed` / 10 `growing` | 新 `seed` 页待下次 ingest 补第二来源；CSDN 接口连续降级 0 有效条目按纪律记「标题未取到」；Harness-Zero「harness distillation」/Salesforce AIforce/UN 面板/MiMo-V2.6/微软 TS→Rust/DolphinBench/DFlash 均来自 arXiv 标题或媒体日报未对照一手来源（见摘要 §待核实）；§8 无活跃 disposition（no-goal 自由生长） |

## 7. 日志归档规则

- `wiki/log.md` 保留最近 30 天记录，更早的按年归档到 `wiki/logs/YYYY.md`（例：`wiki/logs/2026.md`）。
- 归档只搬移，不改写；归档后在本页记一条复核台账。
