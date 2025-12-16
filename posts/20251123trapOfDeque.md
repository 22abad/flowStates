---
title: "Data Structure Design: The Monotonic Queue Pattern"
title_zh: "数据结构设计：单调队列模式"
date: 2025-11-24
author: "Dong Li"
categories: 
  - "Data Structures"
  - "Algorithm Design"
tags:
  - "Monotonic Queue"
  - "Deque"
  - "Java API"
  - "State Management"
summary_en: "Analyzing the Sliding Window Maximum problem. Discussing the critical design decision of storing indices versus values for state management, and clarifying the semantics of Java's Deque API."
summary_zh: "分析滑动窗口最大值问题。讨论用于状态管理的存储下标与存储数值的关键设计决策，并阐明 Java Deque API 的语义。"
---

[EN]
# Data Structure Design: The Monotonic Queue Pattern

## 1. State Management: Index vs. Value
In the **Sliding Window Maximum** problem, a common pitfall is storing **values** (`nums[i]`) in the queue.
However, effective state management requires storing **indices** (`i`). This is the only way to accurately handle the "expiration" state.

*   **Storing Values:** Provides the *magnitude* but lacks the *temporal* context. When the window slides, it is impossible to determine if the maximum value has exited the left boundary (`i - k`).
*   **Storing Indices:** Provides complete state information.
    *   **Magnitude:** Accessible via `nums[index]`.
    *   **Position:** Accessible via `index`.
    *   **Expiration:** Determinable via `index < i - k + 1`.

**Design Principle:** In sliding window contexts, **Index > Value**. The index encapsulates temporal/positional data, which is critical for window validity.

## 2. API Semantics: Head vs. Tail
Java's `Deque` (Double Ended Queue) interface requires precise semantic understanding.

*   **Implicit = Head:** Methods like `peek()`, `poll()`, `remove()` operate on the **HEAD** (the oldest element, the candidate for maximum).
*   **Explicit = Tail:** Methods with the "Last" suffix (e.g., `peekLast()`, `pollLast()`) operate on the **TAIL** (the newest element, the candidate for insertion).

**The Monotonic Queue Logic:**
1.  **Head (Oldest):** Responsible for **Validity**. If the index is expired (out of window), remove it (`poll()`).
2.  **Tail (Newest):** Responsible for **Monotonicity**. If the new element (`nums[i]`) is greater than the tail, remove the tail (`pollLast()`) to maintain the decreasing order.

```java
// 1. Validity Check (Head)
// Note: Storing index allows this check
while (!deque.isEmpty() && deque.peek() < i - k + 1) {
    deque.poll(); // Default operates on Head
}

// 2. Monotonicity Maintenance (Tail)
while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
    deque.pollLast(); // Explicitly operates on Tail
}
```
[END]

[ZH]
# 数据结构设计：单调队列模式

## 1. 状态管理：下标 vs. 数值
在**滑动窗口最大值**问题中，一个常见的陷阱是在队列中存储**数值**（`nums[i]`）。
然而，有效的状态管理需要存储**下标**（`i`）。这是准确处理“过期”状态的唯一方法。

*   **存储数值：** 提供*大小*但缺乏*时间*上下文。当窗口滑动时，无法确定最大值是否已退出左边界（`i - k`）。
*   **存储下标：** 提供完整的状态信息。
    *   **大小：** 通过 `nums[index]` 访问。
    *   **位置：** 通过 `index` 访问。
    *   **过期：** 通过 `index < i - k + 1` 确定。

**设计原则：** 在滑动窗口上下文中，**下标 > 数值**。下标封装了时间/位置数据，这对窗口有效性至关重要。

## 2. API 语义：队头 vs. 队尾
Java 的 `Deque`（双端队列）接口需要精确的语义理解。

*   **隐式 = 队头 (Head)：** 像 `peek()`, `poll()`, `remove()` 这样的方法操作**队头**（最老的元素，最大值的候选者）。
*   **显式 = 队尾 (Tail)：** 带有 "Last" 后缀的方法（如 `peekLast()`, `pollLast()`）操作**队尾**（最新的元素，插入的候选者）。

**单调队列逻辑：**
1.  **队头（最老）：** 负责**有效性**。如果下标过期（超出窗口），将其移除 (`poll()`)。
2.  **队尾（最新）：** 负责**单调性**。如果新元素 (`nums[i]`) 大于队尾，移除队尾 (`pollLast()`) 以保持递减顺序。

```java
// 1. 有效性检查（队头）
// 注意：存储下标允许此检查
while (!deque.isEmpty() && deque.peek() < i - k + 1) {
    deque.poll(); // 默认操作队头
}

// 2. 单调性维护（队尾）
while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
    deque.pollLast(); // 显式操作队尾
}
```
[END]
