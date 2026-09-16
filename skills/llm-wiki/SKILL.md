# LLM Wiki Skill

> 该 Skill 是 LLM 维护本知识库时按需加载的工作流规则。
> 它是 `AGENTS.md`（Schema）的补充细则。**主规范以 `AGENTS.md` 为准**，两者冲突时以 `AGENTS.md` 为最高效力。

## 定位

在 LLM Wiki 模式中：Obsidian 是"IDE"，LLM 是"程序员"，`wiki/` 是"代码库"，`raw/` 是只读的"证据"，`AGENTS.md` 是"编码规范"。
人类在 Obsidian 中浏览、审阅和做判断，LLM 负责编写与维护。

## 触发时机

当用户下达以下指令时加载本 Skill：

- `ingest <素材>` / `消化一下` — 读过素材后编译进 wiki
- `query <问题>` / `查一下 <问题>` — 基于已编译知识回答
- `lint` / `做一次健康检查` — 定期维护检查
- `publish <主题> 为 <类型>` — 产出成品
- `refresh <主题>` — 联网重校验过时内容
- `生长复盘` — 核对生长指标与生命周期状态

## 执行前置

每次操作前，按顺序：

1. 读取 `AGENTS.md`（根目录）确认 Schema 与版本
2. 读取 `wiki/index.md` 了解当前索引与页面清单
3. 读取 `wiki/log.md` 了解最近操作（避免重复劳动）
4. ingest 场景额外读取 `wiki/growth.md`，用「未消化素材」清单确定待处理集合

## 执行后置

每次操作后：

1. 更新 `wiki/index.md`（新增/变更页面必须登记）
2. 更新 `wiki/log.md`（追加一条操作记录）
3. ingest / lint 后核对 `wiki/growth.md` 指标；状态变化时更新 `wiki/lifecycle.md`

---

## 核心机制：自生长

**一次 ingest 若只让 `raw/` 多了一个文件，而 `wiki/` 毫无变化，就是失败。**

Ingest 协议（AGENTS.md §7）：`observe → propose → act → reflect`——**没有 propose 清单不得直接 act；没有 reflect 留痕视为未完成**。

四个必做动作：

| 动作 | 检查点 |
| --- | --- |
| ① 差异检测（observe） | 明确本次要消化哪些素材（而非无目标地遍历全部） |
| ② 增量归并（act） | **先查已有页再决定新建**——命中就补充，不重复造页 |
| ③ 交叉链接（act） | 新页 ≥2 出链、≥1 入链，且登记进 `index.md` |
| ④ 留痕 + 变动分解（reflect） | 追加 `log.md`；刷新 `updated` 与 `status`；缺口写「待核实」；复盘表记 added/revised/merged 与 churn |

**熵减也算生长**：合并重复页、修正错误、补全缺失字段、清理断链、刷新 `stale` 页，与新增页面同等重要。指标只增不减不是目标。持续 churn≈0（只堆不炼）应触发熵减 lint。跨期比较前先过「比较前可靠性」门槛（AGENTS.md §7 Lint）。

对维护习惯的注入，走 **§8 可移除 disposition 实验**（单一、有期限、可撤除；到期做结晶判读），不要直接永久改 Schema。

## 冲突处理

发现不同说法时**不抹平**：保留来源、时间与适用范围（详见 `AGENTS.md` §5）。
证据不足就写进 `## 待核实` 小节并标 `confidence: 低`——**宁可留下问题，不要编造结论**。

## 生命周期

`seed → growing → mature → stale → superseded`（详见 `AGENTS.md` §6）。
新建页面按实际填写 `status`；`superseded` 页面**保留存档不删除**。

---

## 页面质量检查清单

创建或更新任一 wiki 页面时自检：

- [ ] 包含完整 frontmatter（title / aliases / tags / category / created / updated / sources / description）
- [ ] `category` 取值合法（entities | concepts | summaries | comparisons | synthesis）
- [ ] **新建页面**已填 `status`；冲突或证据不足时已填 `confidence` / `scope`
- [ ] 至少与一个其他页面建立 `[[双链]]`（新页 ≥2 出链、≥1 入链）
- [ ] `sources` 指向**真实存在**的 raw 素材（路径逐字核对）
- [ ] `description` 不超过 100 字
- [ ] `updated` 已刷新为当前日期
- [ ] 若有未决分歧，已写 `## 待核实` 小节而非含糊带过

> 元页面（`index.md`、`log.md`、`仪表盘.md`、`growth.md`、`lifecycle.md`）不受 frontmatter 约束——它们不是知识页，是导航与台账。

## 参考

- 行为规范（合同）：`AGENTS.md`
- 操作提示词手册：`PROMPTS.md`
- 生长指标：`wiki/growth.md` ｜ 生命周期台账：`wiki/lifecycle.md`
