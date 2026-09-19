---
title: Harness自进化
aliases:
  - harness self-evolution
  - Agent harness 自改进
  - 权重之外的自进化
tags:
  - AI
  - 智能体
  - 概念
  - 方法论
category: concepts
created: 2026-09-16
updated: 2026-09-19
sources:
  - "[[raw/articles/2026-09-16-Proteus-自进化harness框架-仓库精读]]"
description: 自改进的重心从模型权重移到 harness（prompts/memory/skills/tools/控制循环）；用四相位 episode 与事务契约让 harness 安全地改写自己。
status: seed
confidence: 高
scope: 以 Proteus v0.3.0 设计为蓝本的实验框架视角；非训练时论文综述
---

# Harness自进化

## 定义

**Harness 自进化**：不更新模型权重，而让 agent 在多轮**上下文新鲜**的 episode 中改写自己运行其上的 harness——提示词、记忆、技能、工具与控制循环——并由外部框架负责快照、选择、回滚与测量。

Proteus 的开篇判断：agent 自改进的重心正从 **权重** 移向 **harness**。演化的是「模型怎么被使用」，不是「模型是什么」。

## 核心思想

### 1. 演化对象是一组可声明的 Surface

不是笼统的「让 agent 变聪明」，而是把 harness 拆成**可编辑、可持久、可分别计量**的面（instructions、notes/memory、tools、skills、自身源码…）。Surface 以**数据**声明，测量层不用硬编码名字——换 harness 不用改尺子。

### 2. Episode：四相位 + 上下文新鲜

```
observe  →  propose  →  act  →  reflect
盘点       选一项改进   改自己    验证与定下一步
```

- 每个相位都是**新鲜上下文**；跨 episode 存活的只有 harness 文件（+ 可选的有界操作交接，且交接住在被测快照之外，不算演化记忆）。
- 有目标：四相位都注入 goal 文本；上轮 OBSERVE 可见分只进 observe。
- 无目标：中性探索语言；reflect 记「效果与意外」，不称之为改进；agent 可自立临时目标作为演化状态。

### 3. 事务契约：框架拥有事务，adapter 拥有执行

| 归框架 | 归 adapter |
| --- | --- |
| 相位提示组装、快照、晋升/回滚、评估路由、记录、resume | 相位如何真的跑起来、trace 怎么解析、disposition 装在哪、candidate 怎么校验 |

Staged activation（可选能力）：本 episode 四相位共读**同一冻结 active 快照**；可写 candidate 的编辑要到下 episode 才激活。失败（构建/启动）过不了无模型的边界可行性门 → 回滚，失败树留给下轮当 repair base。**坏掉的演化提案不得控制下一轮运行时。**

### 4. 认识论协议：评估器是证据，不是成功定义

外部 evaluator / benchmark 分数是对演化的**证据**，不自动等于「目标已完成」。窄基准可能完全操作化匹配的目标；宽目标需要 harness 自建额外测试。不为了仪式而堆评估器。无外部目标时不假设目标。

### 5. 归因要求单一可移除扰动

想回答「某个 action preference 有没有塑造 harness」，t=0 只植入**一个**可移除的 Disposition（见 `[[自进化测量标尺]]`）。否则后续分歧无法归因。

## 何时用、何时不用

| 适合 | 不适合 |
| --- | --- |
| 研究：无目标演化会长出什么 / 初始条件是否留下痕迹 | 直接当生产 agent 服务器 |
| 比较不同 harness 面、不同扰动、不同目标条件 | 没有沙箱就让 agent 改自己能执行的代码 |
| 需要可复现的 evolution history（git per episode） | 只想要一个更高 benchmark 分数的黑盒 |

## 与相关概念的边界

- **训练 / 微调**：改权重；Harness 自进化改的是使用权重的脚手架。
- **自愈闭环**（`[[自愈闭环设计]]`）：生产故障下的改码重启通道，目标是可用性；Harness 自进化是实验演化，目标是可测量的形态变化。两者都涉及「agent 改代码」，信任模型与验收标准不同。
- **LLM Wiki 自生长**（`[[自生长知识库实战-苍何]]`、`[[双环知识飞轮]]`）：演化对象是**知识库编译产物**，主体是维护库的 agent+人；Harness 自进化的演化对象是**agent 自己的运行脚手架**。都关心「留下了什么结构化变化」，但被测物不同。
- **RSI（递归自我改进）**：见 `[[每日AI素材-2026-09-15-摘要]]`；Proteus 提供的是其中 harness 路径的实验仪器，不是完整 RSI 理论。
- **外部印证（2026-09-16）**：`[[每日AI素材-2026-09-16-摘要]]` 中 `Tencent/WeKnora`（自维护 Wiki）、`Agentic Societies Need a Social Harness`、`affaan-m/ECC`（agent harness 优化系统）从生产/社区侧印证「演化对象=harness」的判断——自维护知识库、社会协调 harness、harness 性能优化都已是可落地形态。
- **外部印证（2026-09-18）**：`[[每日AI素材-2026-09-18-摘要]]` 从两个方向补强——① `Tencent/WeKnora`（自维护 Wiki）与 `Agents-Flex`（Java 框架**显式支持 LLM Wiki**）再次印证"自生长知识库"已是社区框架能力，不止个人方法论；② `affaan-m/ECC` 把 harness 优化系统化为 skills/instincts/memory/security 的成品，且当日热点补充（PRISM/ExecCritic/MERIT/ResidualAuth/SchemeArena）量化了"memory/tools 不可靠"这一 harness 必须被测量和约束的失败面。

## 参考实例

- `[[Proteus]]`：本概念的实现载体（MIT，v0.3.0）。
- 内置 `minimal` harness 可离线复现「装 review:notes 扰动 → notes 单位数变化可测」的最小演示。

## 相关

- `[[Proteus]]` - 实体
- `[[自进化测量标尺]]` - 怎么测「进化了没有」
- `[[Agent]]` - harness 作为 2026 竞争焦点的语境
- `[[自愈闭环设计]]` - 另一种「agent 改自己」的安全通道
- `[[AI智能体工程方法论]]` - 工程侧原则对照
- `[[每日AI素材-2026-09-18-摘要]]` - 自维护 Wiki 再印证 + harness 可靠性失败面量化
- `[[每日AI素材-2026-09-19-摘要]]` - harness 成为组件级研究对象 + OpenWiki 第三次印证
- `[[LLM-Wiki-vs-RAG]]` - 知识库防腐坏：编译停摆则网络腐坏
- `[[自生长知识库实战-苍何]]` - 自生长知识库的方法论原型
