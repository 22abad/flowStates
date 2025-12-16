---
title: "Algorithmic Robustness: Analyzing a False Positive in KMP Implementation"
title_zh: "算法鲁棒性：分析 KMP 实现中的一个假阳性"
date: 2025-11-23
author: "Dong Li"
categories: 
  - "Algorithm Analysis"
  - "Software Engineering"
tags:
  - "KMP Algorithm"
  - "Debugging"
  - "Edge Cases"
  - "Robustness"
summary_en: "A deep dive into KMP's next array construction. Analyzing a specific bug where 'while(j > 1)' passed test cases despite breaking the algorithm's ability to reset, providing insight into the difference between 'passing tests' and 'correct logic'."
summary_zh: "深入剖析 KMP 算法 next 数组的构建。分析为什么错误的 'while(j > 1)' 依然能通过测试用例，揭示了“通过测试”与“逻辑正确”之间的区别。"
---

[EN]
# Algorithmic Robustness: Analyzing a False Positive in KMP

## 1. Problem Definition
The objective is to determine if a string `s` can be constructed by taking a substring of it and appending multiple copies of the substring together (LeetCode 459).
My approach utilizes the **KMP Algorithm**, specifically analyzing the `next` array (longest equal prefix and suffix).

## 2. Implementation Analysis
During a code review (Space Repetition), I identified a snippet that passed the online judge but contained a critical logical flaw.

```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        int j = 0; 
        int n = s.length();
        int[] next = new int[n];
        next[0] = 0; // Initialization

        for (int i = 1; i < n; i ++) {
            // 🚨 CRITICAL BUG: j > 1
            // Correct Logic: j > 0
            while (j > 1 && s.charAt(i) != s.charAt(j)) {
                j = next[j - 1];
            }
            
            if (s.charAt(i) == s.charAt(j)) {
                j++;
                next[i] = j; // update next array
            }
        }
        
        // Validation logic
        if (next[n - 1] > 0 && n % (n - next[n - 1]) == 0) return true;
        else return false;
    }
}
```

## 3. Root Cause Analysis
Why is `j > 1` incorrect, yet functional in this context?

**The Theoretical Flaw:**
The standard KMP backtracking condition is `while (j > 0 ...)`. This ensures that if a mismatch occurs, the algorithm can backtrack all the way to index 0 (the start of the pattern).
If `j` is limited to `> 1`, the loop terminates prematurely. We lose the ability to backtrack from index 1 to index 0. If `s[i]` mismatches `s[1]`, the algorithm halts, potentially leaving `j` at 1 incorrectly.

**The "False Positive" Mechanism:**
For repetitive strings like "aabaabaab":
*   **Incorrect (`j > 1`) Next Array:** `[0, 1, 1, 2, 2, 3, 4, 5, 6]`
*   **Correct (`j > 0`) Next Array:** `[0, 1, 0, 1, 2, 3, 4, 5, 6]`

Notice that at `i=3`, the incorrect logic kept `next[2]=1`. However, subsequently:
*   At `i=3`, `j` was stuck at 1. `s[3]` matches `s[1]`, so `j` increments to 2. `next[3]=2`.
*   From this point, the pattern synchronizes, and `j` grows correctly.

**Conclusion:**
The condition `j > 1` destroys the algorithm's **Reset Capability**, but preserves its **Growth Capability**.
Since the problem relies on `next[n-1]` (the final state), and repetitive strings tend to produce large `next` values, the intermediate error was masked by subsequent matches. This is a dangerous coincidence, highlighting that **passing unit tests does not prove logical correctness.**

[END]

[ZH]
# 算法鲁棒性：分析 KMP 中的假阳性

## 1. 问题定义
目标是确定字符串 `s` 是否可以通过获取其子字符串并将该子字符串的多个副本附加在一起来构造（LeetCode 459）。
我的方法利用 **KMP 算法**，特别是分析 `next` 数组（最长相等前缀和后缀）。

## 2. 实现分析
在代码审查（间隔重复）期间，我发现了一个片段，它通过了在线评测，但包含一个关键的逻辑缺陷。

```java
// 见上文代码块
```

## 3. 根本原因分析
为什么 `j > 1` 是错误的，但在这种情况下却有效？

**理论缺陷：**
标准的 KMP 回溯条件是 `while (j > 0 ...)`。这确保了如果发生不匹配，算法可以一直回溯到索引 0（模式的开始）。
如果 `j` 被限制为 `> 1`，循环会过早终止。我们失去了从索引 1 回溯到索引 0 的能力。如果 `s[i]` 与 `s[1]` 不匹配，算法就会停止，可能错误地将 `j` 留在 1。

**“假阳性”机制：**
对于像 "aabaabaab" 这样的重复字符串：
*   **错误 (`j > 1`) Next 数组：** `[0, 1, 1, 2, 2, 3, 4, 5, 6]`
*   **正确 (`j > 0`) Next 数组：** `[0, 1, 0, 1, 2, 3, 4, 5, 6]`

注意在 `i=3` 时，错误的逻辑保持 `next[2]=1`。然而，随后：
*   在 `i=3` 时，`j` 卡在 1。`s[3]` 与 `s[1]` 匹配，所以 `j` 增加到 2。`next[3]=2`。
*   从这时起，模式同步，`j` 正确增长。

**结论：**
条件 `j > 1` 破坏了算法的**重置能力**，但保留了其**增长能力**。
由于问题依赖于 `next[n-1]`（最终状态），并且重复字符串倾向于产生大的 `next` 值，中间的错误被随后的匹配掩盖了。这是一个危险的巧合，强调了**通过单元测试并不能证明逻辑正确性。**
[END]
