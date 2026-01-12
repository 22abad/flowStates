---
title: "The Geometric Rebellion: Union-Find vs. The Circle Wall"
title_zh: "几何学的叛逆：并查集与圆圈墙"
date: 2026-01-12
author: "Dong Li"
categories:
  - "Algorithm Analysis"
  - "Computational Geometry"
tags:
  - "Union Find"
  - "Bit Manipulation"
  - "LeetCode 3235"
  - "Functional Interface"
summary_en: "Defying the advice to stick to basics, I tackled the 'Nightmare' problem LeetCode 3235. By fusing the Functional Union-Find template with Bitwise Operations, I transformed a complex geometric blocking problem into a clean connectivity check."
summary_zh: "违背了只做基础题的建议，我向“噩梦”级题目 LeetCode 3235 发起了挑战。通过将函数式并查集模板与位运算融合，我将一个复杂的几何阻挡问题转化为了干净的连通性检查。"
---

<!-- 
Image Prompt:
A technical diagram on a dark digital screen, showing a complex network of circles. Some circles are highlighted in red, forming a jagged, impassable 'wall' that separates the top-left corner from the bottom-right. Faint binary code (bitmasks) hovers over the circles, indicating their connections to the boundaries. The overall style is a mix of a clean geometric blueprint and a gritty, rebellious 'hacker' aesthetic, with annotations scribbled in the margins like battle plans.
-->

![Whiteboard Rebellion|800](https://assets.flowstates.me/2026/20260112geometricRebellion.jpg)
_(The whiteboard at Eolas, 15:21 PM. The evidence of the conquest.)_

[EN]

# The Geometric Rebellion: Union-Find vs. The Circle Wall

## 1. The Provocation

Last night, my AI partner advised a "Force Shutdown," warning me that LeetCode 3235 (Check if Rectangle Corner Is Reachable) was a "Nightmare" suitable only for sober minds. It suggested I start today with a simple Grid DFS.
I woke up at Eolas Lab with a different plan. I didn't want a warm-up. I wanted the boss fight.

## 2. The Logic: Connectivity as a Barrier

The problem asks if we can move from (0,0) to (X,Y) without touching any circles.
A novice sees a pathfinding problem (DFS/BFS).
A master sees a **Connectivity Problem**.
If the circles form a continuous chain that touches both the "Top/Left" boundaries and the "Bottom/Right" boundaries, they form a wall. The path is blocked.

## 3. The Weapon: Functional Union-Find + Bitmasks

I reused the **Functional Union-Find** template (using `IntUnaryOperator` and `BiPredicate`) that I forged yesterday. But `Union` alone isn't enough; we need to know *what* the component is touching.
I introduced a state array `int[] flag`:

* `1 (0001)`: Touches Top
* `2 (0010)`: Touches Bottom
* `4 (0100)`: Touches Left
* `8 (1000)`: Touches Right

When merging two circles `u` and `v`:
`flag[rootU] |= flag[rootV];` (The set inherits all boundary touches).

## 4. The Checkmate

The final check is pure boolean logic. A blockage exists if a set touches:
* Top AND Bottom
* Left AND Right
* Top AND Right
* Left AND Bottom

This reduced a complex geometric simulation into a sequence of `(x1-x2)^2` checks and bitwise `&` operations.
I stood before the whiteboard, 15:21 PM. The nightmare was tamed.

## 5. The Reality Check

The thrill of taming a nightmare problem is a powerful drug. But the victory on the whiteboard is bittersweet. The calendar shows a stark reminder: the CS210 Algorithms exam looms on Wednesday morning. The joy of the LeetCode grind, as exhilarating as it is, must yield to the practical reality of a 40-point paper. The rebellion is over. It's time to return to the fundamentals.

[END]

[ZH]

# 几何学的叛逆：并查集与圆圈墙

## 1. 挑衅

昨晚，我的 AI 伙伴建议我“强制关机”，警告我 LeetCode 3235（检查矩形角落是否可达）是一个只适合清醒头脑的“噩梦”。它建议我今天从简单的网格 DFS 开始。
我在 Eolas 实验室醒来，有了一个不同的计划。我不想要热身。我想要直接打 Boss。

## 2. 逻辑：连通性即屏障

题目问我们是否可以在不接触任何圆圈的情况下从 (0,0) 移动到 (X,Y)。
新手看到的是寻路问题 (DFS/BFS)。
高手看到的是**连通性问题**。
如果圆圈形成了一条连续的链，同时接触到了“上/左”边界和“下/右”边界，它们就形成了一堵墙。道路即被封死。

## 3. 武器：函数式并查集 + 位掩码

我复用了昨天打造的 **函数式并查集** 模板（使用 `IntUnaryOperator` 和 `BiPredicate`）。但光有 `Union` 是不够的；我们需要知道这个集合接触到了*什么*。
我引入了一个状态数组 `int[] flag`：

* `1 (0001)`: 接触顶部
* `2 (0010)`: 接触底部
* `4 (0100)`: 接触左侧
* `8 (1000)`: 接触右侧

当合并两个圆圈 `u` 和 `v` 时：
`flag[rootU] |= flag[rootV];` （集合继承所有的边界接触属性）。

## 4. 将军

最终的检查是纯粹的布尔逻辑。如果一个集合同时接触以下情况，则存在阻挡：
* 上 AND 下
* 左 AND 右
* 上 AND 右
* 左 AND 下

这将一个复杂的几何模拟简化为一系列 `(x1-x2)^2` 检查和位运算 `&` 操作。
下午 15:21，我站在白板前。噩梦被驯服了。

## 5. 现实的钟声

驯服一个噩梦级问题的快感是一种强效的毒品。但白板上的胜利却带着一丝苦涩。日历赫然提醒着：CS210 算法考试就在周三上午。LeetCode 的刷题之旅，尽管爽如登天，也必须让位于 40 分考卷的残酷现实。这场叛逆已经结束。是时候回归基础了。

[END]
