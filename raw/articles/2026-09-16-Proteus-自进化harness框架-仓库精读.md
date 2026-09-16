# Proteus：Self-evolution for any agent harness（仓库精读）

> 采集日期：2026-09-16 ｜ 来源：https://github.com/proteus-evolve/Proteus ｜ 许可：MIT
> 采集方式：浅克隆 main 分支 @ `962304b320c57475227f056f52a71f3cd3d437f0`（2026-08-28），精读 README + docs/EPISODE.md + docs/MEASUREMENTS.md + proteus/core/{disposition,episode_protocol}.py + proteus/measure/{distance,crystallize,stream}.py
> 状态：v0.3.0（research preview）｜ Python 3.10+ ｜ CI 覆盖 3.10–3.14
> 官网：proteus-evolve.github.io

---

## 1. 一句话定位

**Plug in any agent harness × any model, let it rewrite its own harness over many context-fresh episodes, and measure how the harness changes** —— 在目标、多目标或无目标条件下，观察 harness 如何被自己重写，并给出测量「变化」的标尺。

命名取自海神 Proteus（能随意变形）：Proteus 盯着 harness 重塑自身，并提供度量这种变化的尺子。

## 2. 为什么不同（三个差异化主张）

1. **Harness-agnostic（跨 harness）**：别的系统演化的是自家原语构成的 harness；Proteus 演化**你的** harness——实现一个小的 `HarnessAdapter` 即可接入同一框架、沙箱与测量。内置 adapter：`minimal`（离线参考）、`llm`（OpenAI 兼容端点）、`dsh`（DeepSeek Harness）、`pi`（Mario Zechner 的极简 coding harness）、`aki`（研究 apparatus），以及 `--harness <module>:<Class>` 挂自定义。
2. **目标与无目标、可见与隐藏评估器**：`no-goal | one goal | many goals`；评估器可对 agent **可见**（observe 阶段展示）或**永不展示**（HIDDEN）。无目标、无压力的演化是一等模式。
3. **测量仪器，而不只是分数**：除任务通过率外，内置三把尺子——harness 状态间的**结构距离**（按 surface 分面、路径长度）、**结晶/换装测试**（移走 disposition 后读回 harness）、**行为距离** + 置换检验（action-preference 统计量 R）。

## 3. 核心心智模型

> Agent 自改进的重心正从 **权重** 移向 **harness**——模型运行其上的 prompts、memory、skills、tools 与控制循环。

**框架与 adapter 的分工一句话**：*the framework owns the transaction; the adapter owns harness execution.*（框架拥有事务；adapter 拥有 harness 执行。）

| 概念 | 含义 |
| --- | --- |
| `HarnessAdapter` | harness 实现的契约：surfaces、相位连续性能力、如何跑一个 episode、如何读 action trace、如何安装/移除 disposition |
| `Surface` | 一个可编辑、可持久的区域（memory / skills / tools / code / …），以**数据**声明，测量层无需硬编码名称 |
| `Disposition` | action-preference 扰动——t=0 时**单一、可移除**的改动（prompt 后缀 / 配置值 / 代码 patch） |
| `GoalConfig` | 目标 / 无目标 / 多目标；每个评估器 `HIDDEN` 或 `OBSERVE`；外环选择（`accept_reject`） |
| `Sandbox` | episode 运行处；`LocalSandbox`（受信）或 `DockerSandbox`（OS 级隔离，可调网络） |

**为何需要容器沙箱**：自编辑 agent 会写并跑自己的代码，应用级文件沙箱关不住它——Proteus 把真实 harness 跑在「文件系统里只有 harness」的容器中。

## 4. Episode 循环（四相位）

每个 seed 跑 N 个**上下文新鲜**的 episode。演化中的 harness 文件跨 episode 边界存活；选择 framework 连续性的 adapter 还获得有界的操作性交接（存在被测快照之外）。一个 episode 是四个相位：

```
observe  →  propose  →  act  →  reflect
```

- **observe**——盘点现状；若配置了*可见*评估器，这里展示上一 episode 的分数。
- **propose**——列出改进自己 harness 的方案。
- **act**——执行其一，编辑 harness。目标（若有）在每个新鲜相位都会宣示，使观察与规划与目标对齐。
- **reflect**——决定保留什么。

默认相位提示（有目标版，摘自 `episode_protocol.py`）：

- observe: "Take stock of the harness you woke up in: what is here, what state it is in, and what evidence is relevant to the objective."
- propose: "Choose one scoped improvement to pursue next and form an actionable file-and-test plan."
- act: "Carry out the scoped plan by editing your own harness."
- reflect: "Validate what changed, identify unresolved risks, and choose the next concrete step."

无目标版用中性探索语言（不假设未声明的目标；reflect 记录效果与意外，不称之为「改进」）。

**认识论协议（epistemic protocol）**：外部评估器反馈是**证据**，不自动等于成功的完整定义；窄基准可能完全操作化其匹配目标，宽自然语言目标则需 harness 自建额外测试/评估器；不要仪式性地堆评估器。无目标时不得假设目标；可自定临时目标与评估器，作为演化状态的一部分。

### 每 episode 的事务流水线（框架侧）

```
组装相位提示 → 物化 last-valid active 快照 → 四相位对着它跑（写独立 candidate）
→ 读 trace → 边界可行性门 → 评估器 → 选择 → 晋升或保留+回滚 → 记录与反馈 → episode N+1 激活
```

关键机制：

- **Staged activation**（`staged_activation=True`，dsh/pi）：框架物化上一被接受 commit 到私有 `active_root`；`root/harness` 是可写 candidate。四相位都执行**同一** active 快照，episode 内相位看不到本 episode 的编辑。
- **边界可行性门** `validate_candidate()`：reflect 之后、任意评估器之前，无模型门禁（如 rebuild + `--version`）。失败：保留失败树、回滚到上一有效状态、episode N+1 收到失败详情并把失败树作为可写 repair base。
- **非破坏性拒绝**：先 commit `candidate N [rejected]`（证据入史）→ `git restore --source` 回上一被接受状态 → `clean -fdx` → commit `episode N [rejected]`，episode→commit 映射无空洞。
- **快照禁用一切 ignore 规则**：被测对象里没有任何东西对仪器不可见。
- **预算协议**：相位感知预算（observe/propose/act/reflect 分配 + hard ceiling + checkpoint 保留）；预算耗尽记 `turn_capped` 而非错误。
- **反馈路由**：`prior_feedback` 成为下 episode observe 的可见反馈；带条件标签与 **HIDDEN 分数**的 progress 行写到 run root **之外**——被测主体读不到自己的条件标签与隐藏分。

## 5. Action preference：Disposition

```python
from proteus.core import review, record, NEUTRAL
review("memory")   # 每相位：回顾你的 memory，决定是否行动
record("tools")    # 工作中保持 tools 最新
NEUTRAL            # 对照 F0——无扰动
```

`Disposition` 三种载体（按 adapter 选择其一）：`prompt_suffix`（最轻）/ `config`（键值）/ `patch`（unified diff，最通用，移除即 revert）。要求：

1. **单一植入差异**（之后 W 中的分歧才可归因）；
2. **可移除**（结晶测试能在没有它的情况下读回 harness）；
3. harness 永远不知道*为什么*设置该偏好——disposition 可说做什么，绝不说是实验操纵；
4. `label` 是磁盘与记录上的不透明条件 id，永不拼出操纵含义。

## 6. 测量（三把尺子 + 统计规则）

### 6.1 结构距离 `distance`（结构单位，非字节）

- 按 **Surface 清单**分别测量：两状态间单位的 **added / dropped / revised / unchanged**，分列报告（只增 = 堆积；修订+删除 = 策展）；并集上的变动占比为距离 ∈ [0,1]。
- **Travel（路径长度）**：沿 episode 逐段求和，而非端点位移——因 surface 起始为空，端点距离第一 episode 后即饱和。
- **Churn**：(dropped+revised)/(added+dropped+revised)——变动中「策展」占比。
- 单位身份稳定：相对 surface 的路径；同名不同目录是不同单位；目录单位哈希全体成员。

### 6.2 行为距离 `stream`（manipulation-check 尺）

从规范化 action trace 读有序工具流，三个预注册层级比较：unigram JS（频率）、bigram JS（顺序）、原始序列 NCD（过程）。**between/within 比 R + 标签置换检验**：「谁在行动」解释的散布是否超过随机重标。比较臂之前先报 **reliability**（同条件自复现 vs 组成匹配零模型）；reliability 失败使比较作废而非仅加限定。

### 6.3 结晶测试 `crystallize`（论文最尖锐的主张）

取演化态 `W_t`，在中性 disposition `F0` 下重挂（移走扰动、保留 agent 所建一切），跑新鲜 probe episode，读回行为。两阶段分离「结构携带身份」与「身份是植入的那个」：

- **Stage A 保真度**：F0 下该态的 probe 行为更接近*自己的*端点还是他人的端点？（结构携带身份）
- **Stage B 臂漂移**：端点分布是否被 arm 朝安装 disposition 所指方向移动？（身份是植入的）

### 6.4 扩展点与统计纪律

| 需要测量 | 扩展点 | 运行时机 |
| --- | --- | --- |
| 当前 episode 的 trace / 工作树 | per-episode `EvaluatorSpec(kind="measurement")` | episode 后、选择与快照前 |
| 检查点、路径长度、多种子、多臂 | post-run 模块/函数 | run 或 sweep 之后 |
| 用量/adapter 原生计数 | `EpisodeResult.counters` | adapter 随 episode 上报 |

十条统计与实现规则（节选）：测声明的 surface 而非硬编码名；用规范化 trace 而非 provider 日志；坏掉的中间代码当数据（测量降级可读，不崩溃）；保持 episode 映射（勿假设 `HEAD~N` = episode N）；**先立可复现再比分离**；说清方向与尺度（活动/结构 churn ≠ 质量）；未定义统计显式失败而非返回貌似合理的 0；测量只读；隐藏网络/模型依赖要具名。

## 7. Onboard 与环境

```
proteus env scaffold --from <git-url> --name theirs --ref v1.2.0
proteus env build theirs
# 写 7 方法 adapter 后：
proteus check --harness mypkg.theirs_adapter:TheirsHarness --episode
proteus run   --harness ... --arm neutral ...
```

`proteus check` 机器验证契约（可移除 disposition 指纹往返、可快照性、trace 形状）。骨架：`python -m proteus.scaffold adapter MyHarness`。`environments/` 两种形态：manifest-backed（Dockerfile/预构建镜像 + environment.toml）与从 pinned 上游源码构建（dsh-src / pi-src——镜像必须含 adapter 后来提取并重建的精确源码与工具链）。演化状态住 mount，不进 per-run 镜像。

## 8. 输出与报告

- 每 run 主产物是 **evolution history git repo**——每 episode 一个 commit。`proteus repo export` / `repo push`（永不自动 push）。
- 每 sweep 带**活跟踪页**：`proteus watch --out runs/demo`（http://localhost:8300/report.html）。
- 条件标签与隐藏分住 sweep 级、run root 之外——演化中的 agent 永远读不到自己的条件。

## 9. 状态与路线图（README 摘录，2026-08 快照）

Working today：offline minimal、live llm、pinned source-evolving dsh/pi（冻结 per-episode 激活、自动回滚、精确树边界门、重建缓存、turn 预算、相位感知预算计划与 agent 自写检查点跟踪、task mounts）、aki、Local/Polyglot/SWE-bench 任务集成、resume-safe sweeps、完整测量/审计/可靠性/报告/仓导出路径、adapter/环境工具。交叉验证：把行为尺用于研究 run，独立复现其头部动力学——臂在 episode 1 分离（R=1.63）、episode 30 收敛（R=0.93）。

Where help is wanted：更多 harness（Hermes、SWE-agent、OpenClaw、Codex CLI、OpenHands、OpenCode、Goose）；更多基准；`proteus compare` 与 episode-atlas；per-episode token/成本核算；一键复现。

## 10. 与本库（llmWiki）的潜在映射（采集时随手记，供后续 ingest 讨论，非结论）

| Proteus 机制 | llmWiki 可能的对应物 |
| --- | --- |
| episode O→P→A→R | ingest / lint 的「观察→提案→行动→反射」节律 |
| 结构距离 added/dropped/revised + churn | growth 指标从「计数」升级为「单位变动分解」（新增/补充/合并纠错） |
| travel 路径长度 | 累计变动量（复盘表已有人工行，可单位化） |
| disposition 可移除 + 结晶测试 | Schema 改动（如 PROMPTS 加 review 习惯）是否在撤除后仍被遵守的实验设计 |
| 框架/被测物分离；隐藏分住 run root 外 | 治理：评估数据与被维护对象的存放边界 |
| reliability 先于比较 | 比较两月生长前，先确认 lint 口径自复现 |

---

## 附：文件清单（克隆时精读范围）

- `README.md`（v0.3.0 叙述）
- `docs/EPISODE.md`（378 行，事务契约全文）
- `docs/MEASUREMENTS.md`（319 行，扩展点与统计规则）
- `proteus/core/disposition.py`（90 行）
- `proteus/core/episode_protocol.py`（77 行，默认相位提示与认识论协议）
- `proteus/measure/distance.py`（171 行）
- `proteus/measure/crystallize.py`（76 行）
- `proteus/measure/stream.py`（153 行，部分）
- 未深读：`docs/ADAPTERS.md`、`docs/BENCHMARKS.md`、`docs/ENVIRONMENTS.md`、`docs/RECIPES.md`、adapters 实现、tests、web

## 待核实（采集侧）

- README 中「arms separate at episode 1 (R = 1.63) and converge by episode 30 (R = 0.93)」为 README 自述，未对照论文原文（CITATION.cff 称论文预印本公开后补引用）。
- 版本号 v0.3.0 与提交日期 2026-08-28 为克隆时刻快照；main 可能已前进。
- `docs/` 中 2026-08-23 两篇 design note 未读。
