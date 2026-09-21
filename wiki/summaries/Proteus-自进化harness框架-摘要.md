---
title: Proteus-自进化harness框架-摘要
aliases:
  - Proteus仓库精读摘要
  - Proteus-README摘要
tags:
  - AI
  - 工具
  - 方法论
  - 材料摘要
category: summaries
created: 2026-09-16
updated: 2026-09-21
sources:
  - "[[raw/articles/2026-09-16-Proteus-自进化harness框架-仓库精读]]"
description: 仓库精读摘要：Proteus 让任意 agent harness 自我重写并测量变化——四相位 episode、可移除 disposition、结构/行为/结晶三把尺。
status: seed
confidence: 高
scope: MIT 开源研究预览 v0.3.0（克隆快照 962304b，2026-08-28）；论文预印本尚未公开引用
---

# Proteus-自进化harness框架-摘要

> 本页编译 `[[raw/articles/2026-09-16-Proteus-自进化harness框架-仓库精读]]`（GitHub 仓库浅克隆精读，非网页剪藏）。原 raw 含完整机制摘录与文件清单；本页只提炼可复用判断与本库关联。

## 一句话

**Proteus = 任意 agent harness 的自进化实验框架**：接入你的 harness，在「无目标 / 单目标 / 多目标 × 评估器可见/隐藏」条件下跑多轮上下文新鲜的 episode，让 harness 改写自己，并用统一标尺测量「harness 到底变了什么」。

## 三个差异化主张（为什么不是又一个 harness-evolution）

1. **Harness-agnostic**：演化对象是*你的* harness，不是框架自造的原语。实现 `HarnessAdapter` 即接入同一沙箱与测量；内置 minimal / llm / dsh / pi / aki。
2. **目标谱系是一等公民**：`no-goal`（无压力演化）与有目标同权；评估器可 OBSERVE（下轮 observe 可见）或 HIDDEN（只记录）。
3. **测量仪器**：结构距离（按 surface 的 added/dropped/revised + travel）、行为距离（工具流 JS/NCD + 置换检验 R）、结晶测试（移走 disposition 后读回身份）。不是只报任务通过率。

## 最重要的机制判断（可迁移）

| 判断 | 出处要点 |
| --- | --- |
| 演化重心从**权重**移到 **harness**（prompts/memory/skills/tools/控制循环） | README 开篇论点 |
| **框架拥有事务，adapter 拥有执行** | EPISODE.md 分工句 |
| **单一可移除扰动**才能归因：disposition 要求植入差异唯一、可移除、主体不知「为什么」 | disposition.py |
| **只增 = 堆积，修订+删除 = 策展**；churn 分辨这两种生长 | distance.py |
| **travel 用路径长度不用端点位移**（端点距离早饱和） | distance.py |
| **先立 reliability 再比臂**：同条件不自复现则 R 无意义 | stream.py |
| **结晶测试两阶段**：保真度（更像自己的端点）与臂漂移（分布被植入方向移动）分开证 | crystallize.py |
| **隐藏分与条件标签放 run root 之外**——被测主体读不到自己的实验条件 | EPISODE.md 记录节 |
| 评估器反馈是**证据**不是成功定义（认识论协议） | episode_protocol.py |

## Episode 四相位（默认协议）

`observe → propose → act → reflect`。有目标版对齐 objective；无目标版用中性探索语言。事务流水线：组提示 → 物化 last-valid 快照 → 四相对冻结 active 跑（写独立 candidate）→ 读 trace → 边界可行性门 → 评估器 → 选择 → 晋升或非破坏性回滚。

## 与本库关联

- 实体详情：[[Proteus]]
- 概念提炼：[[Harness自进化]]、[[自进化测量标尺]]
- 趋势呼应：[[每日AI素材-2026-09-15-摘要]] 的 RSI 热点；[[Agent]] 的 harness 竞争观察
- 方法论对照：[[双环知识飞轮]]（人/库双环）vs Proteus（harness 单主体自改+测量）；[[自生长知识库实战-苍何]]（LLM Wiki 编译环）

## 待核实

- R=1.63（ep1）→ R=0.93（ep30）为 README 自述交叉验证数字，未对照论文（预印本未公开）。
- v0.3.0 功能清单以克隆快照为准，main 可能已前进；`docs/ADAPTERS.md` 等四篇未深读。
- dsh/pi「source-evolving」的构建细节（冻结依赖、双容器冷启动门）仅按 EPISODE.md 转述，未跑通验证。
