---
title: "The New Year's Eve Protocol: Distinct Subsequences & The Art of Compression"
title_zh: "跨年协议：不同的子序列与压缩之美"
date: 2025-12-31
author: Dong Li
categories:
  - "Algorithm Analysis"
  - "Software Engineering"
  - "Cognitive Shift"
tags:
  - "LeetCode"
  - "Dynamic Programming"
  - "Java"
  - "State Compression"
summary_en: "A deep dive into solving LeetCode 115 (Distinct Subsequences) by optimizing a 2D DP solution to 1D. This post explains the 'ghost variable' technique for preserving the diagonal value and the 'Art of Ignore' philosophy in code design."
summary_zh: "记录攻克 LeetCode 115 (不同的子序列) 的心路历程。从二维动态规划的“脑力便秘”，到利用“幽灵变量”实现一维空间优化的顿悟，并探讨了代码设计中的“无视之道”哲学。"
---

![Digital art illustrating the concept of DP state compression. A large, complex 2D grid of glowing data points is being elegantly funneled and compressed into a single, sleek 1D array of light. A single, brighter point of light, labeled 'pre', acts as a catalyst or a 'ghost variable', guiding the transformation. The background is dark and cerebral, with subtle fireworks in the far distance, hinting at New Year's Eve. Cinematic, 8K, hyper-detailed.|800](https://assets.flowstates.me/2025/20251231newYearsEveProtocol.jpg)

[EN]

# The New Year's Eve Protocol: Distinct Subsequences & The Art of Compression

I spent the final afternoon of 2025 wrestling with **LeetCode 115 (Distinct Subsequences)**. The journey from a 2D DP table to a 1D optimized solution was a two-hour mental squeeze, culminating in a powerful insight.

## How Do You Optimize a 2D DP Solution to 1D When It Has Diagonal Dependencies?

This is a classic DP optimization challenge. The standard 2D solution for this problem relies on the state transition `dp[i][j] = dp[i-1][j] + (s[i-1] == t[j-1] ? dp[i-1][j-1] : 0)`. When compressing to a 1D array, `dp[j]` naturally represents the value from the previous row (`dp[i-1][j]`), but the diagonal value (`dp[i-1][j-1]`) is overwritten when we calculate `dp[j-1]`.

The breakthrough was to introduce a "ghost variable" to act as a time capsule. I named it `pre`.

1.  Before the inner loop calculates `dp[j]`, `dp[j]` holds the value of `dp[i-1][j]` (the vertical neighbor).
2.  The `pre` variable holds the value of `dp[i-1][j-1]` (the diagonal neighbor) from the previous step of the inner loop.

This allows the 2D matrix to collapse into a single array and one rolling variable, perfectly preserving the required historical state.

## Why Is There No `else` Block in Your Final Solution?

This was a conscious design choice rooted in a philosophy I call **The Art of Ignore**.

If `s.charAt(i-1)` does not match `t.charAt(j-1)`, the state transition is `dp[i][j] = dp[i-1][j]`. In the 1D optimized version, this means `dp[j]` should simply retain its value from the previous outer loop iteration. By doing nothing—by omitting the `else` block—we achieve this inheritance implicitly.

This mirrors a broader life strategy: don't waste energy confirming the status quo. Only act when a meaningful event (a character match) occurs. This makes the code cleaner and more intention-revealing.

## The Final Code (10 Lines of Poetry)

Density = Mass / Volume. This solution has infinite density.

```java
class Solution {
    public int numDistinct(String s, String t) {
        // Space Optimization: O(M) instead of O(N*M)
        int[] dp = new int[t.length() + 1];


        for (int i = 1; i <= s.length(); i++) {
            // 'pre' holds the diagonal value dp[i-1][j-1] for the current 'j'.
            int pre = 1;

            for (int j = 1; j <= t.length(); j++) {
                // Store dp[j] (which is dp[i-1][j]) before it's updated.
                int temp = dp[j];

                // If characters match, add the diagonal ways to the vertical ways.
                // dp[j] (new) = dp[i-1][j] + dp[i-1][j-1]
                if (s.charAt(i - 1) == t.charAt(j - 1)) dp[j] = dp[j] + pre;

                // The old dp[j] becomes the 'pre' for the next iteration.
                pre = temp;
            }
        }
        return dp[t.length()];
    }
}
```

[END]

[ZH]

# 跨年协议：不同的子序列与压缩之美



## 第一节：潜得越深，憋得越久

2025 年的最后一个下午，我死磕 **LeetCode 115 (不同的子序列)**。整整两个小时，我处于一种脑力“便秘” (`憋`) 的状态。我非常清楚二维逻辑（`dp[i][j]` 依赖于对角线和正上方），但试图把这个几何结构压扁进一维数组，感觉就像一边开车一边试图折叠地图。

我总是弄丢那个“对角线”的值。每次我更新数组，就把下一步需要的历史数据给覆盖了。

## 第二节：突破——幽灵变量

突破不是来自于写更多的代码，而是来自于理解**时间**。

我意识到 `dp[j]` 代表的是*垂直*邻居（`i-1` 的历史），所以我引入了一个变量 `pre` 充当“时间胶囊”。`pre` 在数据被覆盖前，死死抓住了*对角线*邻居的值。

突然间，庞大的二维矩阵坍缩成了一行整数和一个滚动变量。

## 第三节：哲学——无视之道

我的最终代码里没有 `else` 代码块。

如果 `s.charAt(i)` 不匹配 `t.charAt(j)`，我什么都不做。我让 `dp[j]` 保持原样（直接继承上一行的值）。这就是算法中的**无视之道**：不要浪费 CPU 周期去确认现状。只有在价值匹配时，才采取行动。

这映射了我在爱尔兰的生活策略：无视低维度的噪音（种族歧视、坏天气），只有在遇到高维度的快乐（知识、顿悟）时，才更新我的状态。

## 第四节：代码诗歌（十行）

密度 = 质量 / 体积。这个解法拥有无限的密度。

```java
class Solution {
    public int numDistinct(String s, String t) {
        // 空间优化：从 O(N*M) 降至 O(M)
        int[] dp = new int[t.length() + 1];

        for (int i = 1; i <= s.length(); i++) {
            // “幽灵变量” pre，用于存储 dp[i-1][j-1] 的值
            int pre = 1;

            for (int j = 1; j <= t.length(); j++) {
                int tmp = dp[j]; // 暂存 dp[i-1][j] 的值

                // 若匹配：新值 = 继承上一行的值 + 继承左上角的值
                if (s.charAt(i - 1) == t.charAt(j - 1)) dp[j] = dp[j] + pre;

                // 滚动更新：当前的 tmp (即 dp[i-1][j]) 成为下一次循环的 pre (即 dp[i-1][j-1])
                pre = tmp;
            }
        }
        return dp[t.length()];
    }
}
```

[END]
