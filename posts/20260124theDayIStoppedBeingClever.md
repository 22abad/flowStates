---
title: "The Day I Stopped Being Clever and Started Being Rough"
title_zh: "这一天：我不再耍小聪明，开始变得“粗暴”"
date: 2026-01-24
author: Dong Li
categories:
  - "Reflections"
  - "Mindset"
tags:
  - "Algorithm"
  - "Mindset"
  - "BruteForce"
  - "Growth"
summary_en: "A reflection on a shift in mindset. The author explains the return from chasing elegant but fragile solutions to embracing brute-force methods. Because brute-force thinking is the most direct, fundamental instinct, it's the only source for building true confidence, upon which all advanced techniques must be built."
summary_zh: "一篇关于心态转变的反思。作者阐述了为何要从追求优雅但易碎的方案，回归到拥抱“暴力”解法。因为暴力思维是最直接、最底层的本能，是建立真正自信的唯一来源，而所有高级技巧都必须建立在这个坚实的基础上。"
---

<!--
Image Prompt:
A programmer in a gritty, workshop-like environment, surrounded by computer parts and monitors. On the main screen, a complex, elegant but shattered glass-like structure representing an over-engineered solution is fading away. In its place, the programmer is building a new structure with their bare hands, made of solid, rough, glowing blocks of code, symbolizing a robust brute-force approach. The style is realistic with a touch of digital fantasy, emphasizing the texture of the rough blocks and the determined expression of the programmer. The overall tone is one of breakthrough and a return to first principles. --ar 21:9 --v 6.0
-->

![Brute Force Foundation|800](https://assets.flowstates.me/2026/20260124theDayIStoppedBeingClever.jpg)

[EN]

# The Day I Stopped Being Clever and Started Being Rough

## 1. The Descent: The Illusion of Elegance

I started the day trying to be a "Senior Engineer" before I was even a competent "Junior". I was obsessed with one-liners, Streams, and BigInteger, thinking complexity equaled competence.

The reality checked me hard. I stumbled on basic Easy problems (LC 242, LC 66) because I was over-engineering. My "fancy" logic collapsed under the weight of simple constraints. I realized my hand-brain gap was widening because I refused to get my hands dirty.

## 2. The Breakthrough: Embracing the Brute Force

The turning point came with the "Survival Protocol". I accepted that a working brute-force solution is infinitely better than a broken elegant one.

*   **XOR Magic:** I learned that `^` isn't just math; it's a "Cancellation Game". (LC 136, 389)
*   **Physical Models:** I stopped memorizing code and started visualizing physics. The "Balance Scale" for Pivot Index (LC 724) and the "Look Ahead" for Roman Numerals (LC 13).

The climax was solving LC 1572 (Matrix Diagonal Sum). I initially solved it with a "dumb" brute force loop, then optimized it to $O(N)$ using my own mathematical intuition (subtracting the center at the end).

## 3. The Philosophy: Brute Force as the Sole Source of Confidence

I used to think algorithms were about genius invention. Today, I realize they are first and foremost about survival.

I am still in the "brute force phase," but this is the only way for me to build confidence. Because brute-force thinking—this raw power in problem-solving—is the most direct and deepest of instincts. Higher-level techniques, the elegant thoughts, are like the neocortex, acquired through later learning. They are sophisticated, yet fragile and fleeting. Only the deep-seated "violence" of brute force can provide an endless and singular source of confidence when facing the unknown and the difficult.

My "dumb" code is my foundation. My "fancy" optimization is just the result of seeing enough patterns. I am not a genius; I am an "ordinary person" who embraces the brute force phase as a foundation to eventually reach optimization.

What we call "advanced" is merely brute force, refined a thousand times. Wobbling is growing. Surviving is thriving.

[END]

[ZH]

# 这一天：我不再耍小聪明，开始变得“粗暴”

## 1. 下坠：优雅的幻觉

今天一开始，我还没学会走，就想当“高级工程师”。我痴迷于一行流代码、Stream 流和 BigInteger，误以为把代码写得复杂就是能力强。

现实狠狠打了我一巴掌。我在简单的 Easy 题（LC 242, LC 66）上栽了跟头，因为我过度设计了。我那些“花哨”的逻辑在简单的约束条件下崩塌了。我意识到，我的“手脑差距”之所以在拉大，是因为我拒绝弄脏自己的手。

## 2. 突破：拥抱暴力美学

转折点来自于“生存协议”。我接受了一个事实：一个能跑通的暴力解法，胜过一万个跑不通的优雅解法。

*   **异或魔法:** 我学会了 `^` 不仅仅是数学，它是“消消乐”游戏。(LC 136, 389)
*   **物理模型:** 我不再死记硬背代码，开始想象物理场景。比如寻找中心下标时的“天平模型”（LC 724），以及罗马数字转换时的“向前看”逻辑（LC 13）。

高潮出现在解决 LC 1572（矩阵对角线和）时。我最初用“笨”办法搞定，然后利用我自己的数学直觉（最后减去中心点）把它优化到了 $O(N)$。

## 3. 哲学：暴力是唯一的信心来源

我曾经以为算法关乎天才的发明。今天，我意识到它首先关乎生存。

我仍然处在“暴力”阶段，但这正是我累积信心的唯一方法。因为暴力思维，这种解题的“蛮力”，是最直接、最深处的本能反应。高级的技巧和优雅的思路，就像是后天学习获得的新皮层，虽然精妙，但易碎且不持久。只有深植于本能的“暴力”，才能在面对未知和困难时，给我提供那无尽且唯一的信心。

我的“笨”代码是我的地基。我的“洋气”优化只是因为我见过了足够多的模式。我不是天才；我是一个正视并拥抱暴力阶段，并以此为根基，一步步走向优化的“普通人”。

所谓高级，不过是经过千锤百炼的暴力。摇晃就是在生长。活下来就是赢。

[END]