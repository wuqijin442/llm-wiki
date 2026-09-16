---
title: Proteus
aliases:
  - proteus-evolve
  - Proteus harness evolution
tags:
  - AI
  - 工具
  - 框架
  - 智能体
category: entities
created: 2026-09-16
updated: 2026-09-16
sources:
  - "[[raw/articles/2026-09-16-Proteus-自进化harness框架-仓库精读]]"
description: MIT 开源的 harness 自进化实验框架：任意 agent 接入后自我重写，用结构/行为/结晶三把尺测量变化。
status: seed
confidence: 高
scope: 研究预览 v0.3.0（快照 2026-08-28）；面向实验与测量，不是生产 agent 运行时
---

# Proteus

## 概述

[proteus-evolve/Proteus](https://github.com/proteus-evolve/Proteus)：**Self-evolution for any agent harness. Plug in. Evolve. Measure.** 命名取自能随意变形的海神。MIT，Python 3.10+，研究预览。

核心问题不是「刷高某基准」，而是：**自进化的 harness 实际会长成什么样？初始条件会不会留下永久痕迹？**

## 关键属性

| 项 | 值 |
| --- | --- |
| 定位 | harness 自进化实验框架 + 测量仪器 |
| 演化对象 | 任意 harness 的可编辑面（instructions / notes / tools / skills / 自身源码…） |
| 接入方式 | 实现 `HarnessAdapter`（约 7 方法）；`proteus check` 契约校验 |
| 内置 harness | `minimal`（离线）、`llm`、`dsh`、`pi`、`aki` |
| 运行隔离 | `LocalSandbox` / `DockerSandbox`（自编辑代码必须容器隔离） |
| 主产物 | 每 episode 一 commit 的 evolution history git repo |
| 版本 | v0.3.0 research preview（本库采集时快照） |

## 架构一句话

**框架拥有事务（快照/晋升/回滚/记录/resume），adapter 拥有 harness 执行（相位怎么跑、trace 怎么读、disposition 装哪）。**

```
Run config → 框架组相位提示 → HarnessAdapter.run_episode → harness/（被测）
                                    → task/（基准工作区，不被测）
                                    → 原生 logs → 评估器 → 选择+快照 → 循环
```

## 核心对象

- `Surface`：可编辑持久面，**以数据声明**，测量层零硬编码
- `Disposition`：单一可移除的 action-preference 扰动（见 `[[自进化测量标尺]]`）
- `GoalConfig`：目标/无目标/多目标 × HIDDEN/OBSERVE × accept_reject
- Episode：`observe → propose → act → reflect`（见 `[[Harness自进化]]`）

## 设计要点（读代码时印象最深的）

1. 快照 **禁用一切 ignore 规则**——被测物对仪器必须全可见。
2. staged adapter：四相位共读**同一冻结 active**，本 episode 的编辑只进 candidate，下 episode 才激活；失败树作下一可写 repair base。
3. 非破坏性拒绝：rejected 候选先入史再回滚，episode→commit 映射无空洞。
4. 隐藏评估分与条件标签写在 run root **之外**，防主体自读实验条件。
5. 边界可行性门（rebuild + `--version` / 冷启动）在任意评估器之前，坏代码不得被执行评估。

## CLI 速览

```
proteus run --harness minimal --arm neutral --arm review:notes --seeds 4 --episodes 8 --out runs/demo
proteus measure --harness minimal --out runs/demo
proteus watch --out runs/demo          # 活跟踪页
proteus check --harness my.adapter:Cls --episode
```

## 相关

- `[[Harness自进化]]` — 核心概念（演化对象从权重到 harness）
- `[[自进化测量标尺]]` — 结构距离 / 行为距离 / 结晶测试
- `[[Proteus-自进化harness框架-摘要]]` — 仓库精读摘要
- `[[Agent]]` — 2026 harness 竞争语境
- `[[每日AI素材-2026-09-15-摘要]]` — 同日 RSI 热点
- `[[双环知识飞轮]]` — 另一种「自生长」范式（人+库），可对照
