---
title: "Algorithmic Patterns: Deconstructing Tree Traversal Logic"
title_zh: "算法模式：解构树遍历逻辑"
date: 2025-11-30
author: "Dong Li"
categories:
  - "Algorithm Analysis"
  - "Data Structures"
tags:
  - "Binary Tree"
  - "Stack"
  - "Pattern Recognition"
summary_en: "A comparative analysis of iterative tree traversal methods. Deconstructing the difference between Standard Iteration (Path Timing) and Unified Iteration (State Marking) to clarify the logic of result collection."
summary_zh: "对迭代树遍历方法的比较分析。解构标准迭代（路径时机）和统一迭代（状态标记）之间的区别，以阐明结果收集的逻辑。"
---

[EN]
![Stack Philosophy|600](https://assets.flowstates.me/2025/20251130stackPhilosophy.jpg)

# The Complexity of State Management

In the domain of Binary Tree iteration, the logic often appears fragmented to the novice.

- **Preorder** utilizes a straightforward stack.
- **Inorder** necessitates a pointer for leftward traversal.
- **Postorder** frequently requires dual stacks or reverse logic.
  The existence of three distinct implementation patterns for a single data structure introduces unnecessary cognitive load.

The **Unified Iteration (Marker Method)** offers a consistent logic for all traversals.
The fundamental distinction lies in the **"Logic of Collection."**

## 1. Standard Iteration: Implicit State

In Standard Iteration (specifically Inorder), the Stack serves primarily to **record the path**.
We push nodes during traversal.

- **The Rule:** Value collection (adding to `res`) occurs only upon **popping** from the stack _after_ reaching a left-side terminus.
- **The Complexity:** Reliance on **Timing**.
  * Preorder: Collect *before* push.
  * Inorder: Collect _after_ pop.
  * Postorder: Timing is highly specific.
  Because the "Collection Event" varies temporally, the code structure *must* vary structurally.

```java
// Standard Inorder: Logic depends on pointer movement
while (curr != null || !stack.isEmpty()) {
    if (curr != null) {
        stack.push(curr);
        curr = curr.left; // Move Left
    } else {
        curr = stack.pop();
        res.add(curr.val); // <--- Collection Timing: After left is done
        curr = curr.right;
    }
}
```

[END]

[ZH]
![Stack Philosophy|600](https://assets.flowstates.me/2025/20251130stackPhilosophy.jpg)

# 状态管理的复杂性

在二叉树迭代领域，逻辑对新手来说往往显得支离破碎。

- **前序** 利用一个简单的栈。
- **中序** 需要一个指针向左遍历。
- **后序** 经常需要双栈或反转逻辑。
  单一数据结构存在三种不同的实现模式，引入了不必要的认知负荷。

**统一迭代法（标记法）** 为所有遍历提供了一致的逻辑。
根本区别在于**“收集的逻辑”**。

## 1. 标准迭代：隐式状态

在标准迭代（特别是中序）中，栈主要用于**记录路径**。
我们在遍历过程中压栈。

- **规则：** 值收集（加入 `res`）仅发生在左侧终点后从栈中**弹出**时。
- **复杂性：** 依赖**时机**。
  * 前序：压栈前收集。
  * 中序：弹栈后收集。
  * 后序：时机非常具体。
  因为“收集事件”在时间上各不相同，代码结构*必须*在结构上有所不同。

```java
// 标准中序迭代：逻辑依赖指针移动
while (curr != null || !stack.isEmpty()) {
    if (curr != null) {
        stack.push(curr);
        curr = curr.left; // 向左移动
    } else {
        curr = stack.pop();
        res.add(curr.val); // <--- 收集时机：左侧完成后
        curr = curr.right;
    }
}
```
[END]
