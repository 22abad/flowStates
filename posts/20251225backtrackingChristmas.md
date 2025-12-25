---
title: "Backtracking Christmas: Logic Pruning and the Algorithm of Inner Peace"
title_en: "Backtracking Christmas: Logic Pruning and the Algorithm of Inner Peace"
title_zh: "回溯圣诞：逻辑剪枝与内心的算法"
date: 2025-12-25
author: "Dong Li"
categories:
  - "Algorithm Analysis"
  - "Cognition"
  - "Personal Growth"
tags:
  - "Backtracking"
  - "LeetCode"
  - "Java"
  - "Mindset"
  - "Rebirth"
summary_en: "On this Christmas Day, 110 days into my coding journey, I engaged in a deep dive into Backtracking algorithms. This post clarifies the critical distinction: 'Tree Branch' handles validity (depth), while 'Tree Level' is the main battlefield for deduplication (width). It also reflects on the paradox of focus—how I found absolute silence in a noisy Christmas party, and why true strength is formless."
summary_zh: "在转码第110天的圣诞节，我深入剖析了回溯算法中“树枝”与“树层”的去重逻辑。更重要的是，我反思了关于“专注”的伪命题：我不发朋友圈，却在嘈杂的圣诞聚会上找到了内心的宁静。真正的修炼，不是寻找安静的温室，而是练就一种收放自如、大道无形的心智。"
---

<!-- Image Prompt: A cozy yet high-tech Christmas setting in a large warm study room. A glowing holographic Christmas tree is constructed from binary tree nodes and edges, illuminating a desk with a laptop displaying Java code. Outside the window, a quiet snowy night in an Irish town. The atmosphere is peaceful, intellectual, and focused. Style: Cyberpunk meets Hygge, warm lighting, sharp focus, 8k. -->
![backtrackingChristmas|600](https://assets.flowstates.me/2025/20251225backtrackingChristmas.jpg)

[EN]
# 🎄 Backtracking Christmas: Logic Pruning and the Algorithm of Inner Peace

Date: December 25, 2025 | Location: Maynooth, Ireland | Mood: Calm & Logical

## The "Bank Holiday" Silence

Today is Christmas, December 25th, 2025. Outside my window in Maynooth, the world is quiet. It is a "Bank Holiday," a day for families and turkey.

But for me, sitting in my new "cockpit"—a suite I rented to optimize my study efficiency—it is Day 110 of my "Rebirth." From that first landing on September 7th to today, 110 days have passed. It is an extraordinary day where I gift myself the clarity of logic.

## The Technical Deep Dive: Tree Levels vs. Tree Branches

My focus today was on Backtracking algorithms (LeetCode 40, 46, 47). For a beginner, the distinction between "Tree Level" (Horizontal) and "Tree Branch" (Vertical) is the key to mastering deduplication.

Let's refine the mental model:

-   **The Tree Branch (Vertical / Depth):** This is about Validity.
    -   It represents the path growing deeper.
    -   Logic: We typically don't need to "deduplicate" values here (a parent and child can have the same value). Instead, we enforce rules like "don't pick the same index twice" (using `used[i]` in Permutations) or "don't look backwards" (using `start_index` in Combinations).

-   **The Tree Level (Horizontal / Width):** This is the Battlefield for Deduplication.
    -   It represents the choices at the current step.
    -   Logic: Every recursion level has its own for loop. If `nums[i]` is the same as `nums[i-1]`, and we start a new branch from here, we will produce an identical result set to the one we just finished. This is where we must prune.

## The Breakthrough: The Pruning Arsenal

In Permutations II (LeetCode 47), handling duplicates (e.g., `[1, 1, 2]`) requires distinct strategies.

```java
// Logic for Permutations II (Deduplication)
Arrays.sort(nums); // Prerequisite: Sort to group duplicates

for (int i = 0; i < nums.length; i++) {
    // --- TREE LEVEL (Deduplication) ---
    // "Have I already started a branch with this value at this level?"
    // !used[i-1] means the previous identical number was used, finished, and backtracked.
    // It belongs to the history of this level. We skip to avoid repetition.
    if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) {
        continue;
    }
    
    // --- TREE BRANCH (Validity) ---
    // "Is this specific element already in my current path?"
    // used[i] ensures we don't pick the exact same physical element twice in one branch.
    if (used[i]) continue;
    
    // ... backtracking recursion ...
}
```

Key Takeaway:

-   Tree Branch: Controls the `idx` (start index) or `used` status to ensure we move forward correctly.
-   Tree Level: Uses `nums[i] == nums[i-1]` (often combined with `!used[i-1]`) to ensure we don't repeat the same choice at the same position.

## The Philosophy: Formless Focus

Why tackle LeetCode on Christmas?

I don't post on social media anymore. I don't even check it. To me, it is often a source of negativity. But ironically, my deliberate avoidance proves that I still care. It proves I am guarding myself against Jealousy (fearing others' success highlights my failure) while trying to cultivate Envy (admiring the possibility of success).

### The Cafeteria Epiphany

But today, I felt a shift. A transition from "avoiding noise" to "ignoring noise."

It happened today at the Christmas Pizza Party organized by the school for international students in the Arts Building. It was chaotic—loud chatter, the smell of pizza, and festive excitement. Yet, I was grinding through LeetCode, completely unaffected. The noise became white noise; the world faded into the background.

I used to believe in the "Library Fallacy"—that I needed a perfectly silent environment to focus. I realized today that this is a pseudo-proposition.

"I can't focus because I'm not engaged."
"I can't engage because I can't focus."

It seems like a deadlock. But the key isn't the environment; it's the Subject. Choosing the right problem—one that matches my skill level and sparks curiosity—is more important than the method. But without the method, I wouldn't find the right problem.

### The "Great Way"

Breaking this deadlock requires powerful mental state management.

Today, I realized I have achieved a form of "Formless Way" (Dao). I can expand my awareness to chat with my landlady, and contract it instantly to debug a recursion tree. True strength isn't about finding a quiet room; it's about the ability to retract and release one's focus at will.

Merry Christmas. The code is compiled. The logic is sound. My mind is free.

[END]

[ZH]
# 🎄 回溯圣诞：逻辑剪枝与内心的算法

日期：2025 年 12 月 25 日 | 地点：爱尔兰·梅努斯 | 心情：宁静且逻辑清晰

## “法定假日”的静谧

今天是 2025 年 12 月 25 日，圣诞节。梅努斯（Maynooth）的窗外一片寂静。对于大多数人来说，这是传统的“Bank Holiday”，是家人团聚吃火鸡的日子。

但对我而言，坐在我新租的“高达驾驶舱”（为了优化学习环境而租的套间）里，这是我“转生”（Rebirth）后的第 110 天。从 9 月 7 日凌晨落地，到今天，整整 110 个日夜。这是一个非凡的日子——我决定送给自己一份礼物：逻辑的通透。

## 技术深潜：树层才是去重的主战场

今天的核心任务是攻克回溯算法（Backtracking），特别是排列组合中的去重逻辑（LeetCode 40, 46, 47）。以前我总搞不清“树层”（Tree Level）和“树枝”（Tree Branch）的关系，今天终于理顺了：

-   树枝（Tree Branch / 纵向）：这是合法性（Validity）的控制。
    -   它代表递归的深度。
    -   通常不需要去重（父节点和子节点的值可以相同，比如 [1, 1]）。我们要控制的是“不选自己”（用 `used[i]`）或者“不走回头路”（用 `start_index`）。

-   树层（Tree Level / 横向）：这才是去重（Deduplication）的主战场。
    -   它代表每一层递归里的 for 循环。
    -   每一层都有自己的 i 循环。如果当前位置选了“1”，下一个循环又选了个长得一样的“1”，那么这条分支延伸下去的结果一定是一样的。这就是我们要剪掉的。

## 顿悟时刻：剪枝的组合拳

在 全排列 II（LeetCode 47）中，我们面对的是不同维度的控制：

```java
// 全排列 II 的逻辑
Arrays.sort(nums); // 前提：排序，让相同的元素聚在一起

for (int i = 0; i < nums.length; i++) {
    // --- 树层去重 (Tree Level) ---
    // 核心目标：同一层级，不要选重复的“领头羊”。
    // !used[i-1] 说明前一个相同的数字已经“下班”了（Backtracked），属于历史。
    // 如果现在还选它，就是在重复历史。
    if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) {
        continue;
    }
    
    // --- 树枝合法性 (Tree Branch) ---
    // 核心目标：同一条路径上，不要重复选同一个物理位置的元素。
    if (used[i]) continue;
    
    // ... 递归回溯 ...
}
```

核心逻辑：树枝控制深浅和路径有效性，树层控制广度和结果唯一性。

## 哲学反思：大道无形与专注的伪命题

### 朋友圈的“反向证明”

我不发朋友圈，甚至早就关闭了入口。对我来说，那里往往是负能量的温床，是无效信息的集散地。但在这个圣诞节，我意识到：我不刷朋友圈，恰恰说明我在乎。这种刻意的物理隔绝，是因为我潜意识里在防御。我在防御 Jealousy（嫉妒）——那种害怕别人的光芒映照出自己暗淡的恐慌；同时我在小心翼翼地呵护 Envy（羡慕）——那种承认别人美好并渴望自己也能做到的希冀。回避，是因为还未强大到可以从容面对。

### Arts Building 的顿悟

但今天，我觉得自己的心态完成了一次跃迁。就在今天，在 Arts Building 学校为留学生组织的圣诞联欢 Pizza 小聚会上。

那是午餐高峰期，周围人声鼎沸，大家都在开心地吃着披萨聊天。在那样嘈杂的环境里，我居然没有任何影响地刷着 LeetCode。周围的一切都成了虚化的背景音（White Noise），我的世界里只有代码和逻辑。

那一刻我明白，我以前对“环境”的要求，对“专心致志”的定义，都是伪命题。

我们常陷入一个死循环：
“因为没看进去，所以觉得环境吵，没法专心。”
“因为没法专心，所以根本看不进去。”

### 破解死锁：收放自如

这是一个表面上相互嵌套的死锁（Deadlock）。但破解它的钥匙，不在于寻找一个绝对安静的图书馆，而在于**选题（Subject）与心智（Mindset）**的匹配。选题比方法更重要，因为只有匹配了难度与兴趣，才能引发心流；但没有方法，又选不到好的题目。

在适当的时机，利用强大的心智去强行打破这个循环，这就是我们“修炼”的目的。

真正的强，不是在真空中飞行，而是在风暴中依然能握稳方向盘。能在 Arts Building 的喧嚣中入定，也能在房东的闲聊中抽离。收放自如，谓之大道无形。

圣诞快乐。代码已编译。心智已升级。

[END]
