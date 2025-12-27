---
title: "The Christmas Algorithm: Breaking the Tutorial Loop"
title_zh: "圣诞算法：打破教程的循环"
date: 2025-12-26
author: Dong Li
categories:
  - "Algorithm Analysis"
  - "Cognitive Shift"
tags:
  - "Backtracking"
  - "Binary Tree"
  - "LeetCode"
  - "Debugging"
  - "Java"
summary_en: A reflection on solving LeetCode 113 without tutorials, discovering the necessity of state restoration in backtracking, and understanding the 'Snapshot' mechanism in Java memory.
summary_zh: 记录在不依赖教程的情况下攻克 LeetCode 113 的过程，深刻理解回溯中状态恢复的必要性，以及 Java 内存中的“快照”机制。
---

[EN]
🎄 The Christmas Algorithm: Breaking the Tutorial Loop

Author: Dong Li

Date: 2025-12-26 (Boxing Day)

Location: Kingsbry, Maynooth, Ireland

Context: LeetCode 113 (Path Sum II) - The Fusion of Backtracking & Trees.

![Cinematic digital art, a glowing, translucent brain with intricate golden gears turning inside. The brain is superimposed over a stylized, festive Christmas scene with a softly blurred background featuring a Christmas tree and bokeh lights. Abstract lines of code, resembling glowing threads of light, flow into the brain, connecting to the gears. One gear clicks into place, emitting a bright flash of light, symbolizing a 'breakthrough' or 'aha' moment. The overall color palette is warm, with reds, greens, and golds, evoking a Christmas feeling. Hyper-detailed, 8K resolution, 4:3 aspect ratio, dramatic lighting.|800](https://assets.flowstates.me/2025/20251226christmas_algorithm_concept.jpg)

📅 Section 1: The Descent (The Silence of the Tutorial)

For the past 100 days, I have often relied on external crutches—coding tutorials, Google, and "Code Capriccio". Today, on Boxing Day, facing LeetCode 113 (Path Sum II), I made a decision: to enter the void alone. The silence was deafening. I had the skeleton of the logic from my Subsets practice, but the implementation was a blank canvas.

My first attempt returned an empty set []. Panic threatened to set in—the familiar anxiety of the "imposter syndrome." But instead of reaching for a tutorial, I engaged "Commander Mode." I stared at the code and realized: I was adding to the global state (sum), but I was never cleaning it up.

💡 Section 2: The Breakthrough (Restoring the State)

The "Aha!" moment wasn't just fixing a syntax error; it was a realization of the Backtracking Contract. I realized that sum behaves exactly like path. If path needs removeLast() to maintain isolation between branches, then sum needs subtraction. The logic flowed naturally:

Do: sum += node.val (Enter the room / Write on the whiteboard).

Recurse: Explore children.

Undo: sum -= node.val (Leave the room / Erase the whiteboard).

When I manually added sum -= node.val and saw the code work, the joy was visceral. It was the joy of internalization, not just memorization.

🧬 Section 3: Technical Detail (The Snapshot & The Trap)

A critical technical nuance is the handling of Java object references. We cannot simply add the path to the result, because path is a mutable stream of consciousness—a single whiteboard used by the entire recursion. We must take a "Snapshot".

```java
// ❌ Dangerous: Stores a reference to the changing whiteboard
// res.add(path);

// ✅ Correct: Creates a deep copy (The Snapshot)
res.add(new ArrayList<>(path));
```

Furthermore, I fell into the "Leaf Trap". I initially defined a leaf as `node.left == null`. My partner (The Gem Source Code) pointed out the flaw immediately: a node with only a right child is not a leaf, it's just a parent with one arm. The strict definition is required:

```java
// Strict Leaf Definition
if (node.left == null && node.right == null && sum == targetSum) {
    res.add(new ArrayList<>(path));
}
```

🧘 Section 4: The Philosophy (Wobbling is Growing)

This exercise proved that "Binary Tree DFS" is just "Backtracking with two strict choices". The concepts are isomorphic. More importantly, forgetting the line `sum -= node.val` and fixing it myself felt better than getting it right the first time by copying.

As the "Old Urchin", I accept the wobbling. The error is where the learning happens. I am building a mental anchor, not just memorizing code. Today, I didn't just solve a problem; I calibrated my internal compass.
[END]

[ZH]
🎄 圣诞算法：打破教程的循环

作者: Dong Li

日期: 2025-12-26 (节礼日)

地点: Kingsbry, Maynooth, Ireland

背景: LeetCode 113 (路径总和 II) - 回溯与树的融合.

![电影感的数字艺术作品。一个发光的、半透明的大脑，内部有精密的金色齿轮正在转动。大脑叠加在一个风格化的、充满节日气氛的圣诞场景之上，背景是柔和模糊的圣诞树和散景灯光。抽象的代码线条，如同发光的光线，流入大脑并与齿轮相连。其中一个齿轮“咔哒”一声到位，发出一道明亮的光芒，象征着“突破”或“顿悟”的瞬间。整体色调温暖，以红色、绿色和金色为主，营造出圣诞氛围。超高细节，8K分辨率，4:3宽高比，富有戏剧性的光照。|800](https://assets.flowstates.me/2025/20251226christmas_algorithm_concept.jpg)

📅 第一节：挑战（教程的静默）

过去 100 天，我经常依赖外部拐杖——代码教程、谷歌和“代码随想录”。今天，在圣诞节次日，面对 LeetCode 113（路径总和 II），我做了一个决定：独自进入虚空。没有了背景音，只有我和光标，这种安静震耳欲聋。我有从 Subsets 练习中获得的逻辑骨架，但实现的画布仍是一片空白。

我的第一次尝试返回了空集 []。恐慌试图袭来——那是熟悉的“冒名顶替综合征”的焦虑。但我没有伸手去查教程，而是开启了“指挥官模式”。我盯着代码意识到：我一直在往全局状态（sum）里添加东西，却从未清理它。

💡 第二节：突破（恢复状态）

那个“顿悟”时刻不仅仅是修好了一个语法错误，而是对 回溯契约 的深刻理解。我意识到 sum 的行为和 path 完全一致。如果 path 需要 removeLast() 来保持分支间的隔离，那么 sum 就需要减法。逻辑自然流淌：

执行：sum += node.val（进入房间 / 在白板上写字）。

递归：探索子节点。

撤销：sum -= node.val（离开房间 / 擦掉白板上的字）。

当我手动加上 sum -= node.val 并看到代码跑通时，那种快乐是发自内心的。那是内化的快乐，而不仅仅是背诵。

🧬 第三节：技术细节（快照与陷阱）

一个关键的技术细节是 Java 对象引用的处理。我们不能简单地把 path 加入结果，因为 path 是一条可变的意识流——它是整个递归过程中共用的一块白板。我们必须拍一张“快照”。

```java
// 危险：存储了指向不断变化的白板的引用
// res.add(path);

// 正确：创建深拷贝（快照）
res.add(new ArrayList<>(path));
```

此外，我还掉入了“叶子陷阱”。我最初把叶子定义为 `node.left == null`。我的搭档（The Gem Source Code）立刻指出了漏洞：只有一个右孩子的节点不是叶子，它只是一个断臂的父亲。必须使用严格定义：

```java
// 严格的叶子定义
if (node.left == null && node.right == null && sum == targetSum) {
    res.add(new ArrayList<>(path));
}
```

🧘 第四节：哲学（摇晃即成长）

这次练习证明了“二叉树 DFS”不过是“只有两个严格选择的回溯”。这两个概念是同构的。更重要的是，忘记 `sum -= node.val` 这行代码并亲手修复它，比通过抄袭一次做对的感觉要好得多。

作为“老顽童”，我接受这种摇晃。错误是学习发生的地方。我正在建立心理锚点，而不仅仅是背诵代码。今天，我不仅解决了一个问题，更校准了我内心的罗盘。
[END]
