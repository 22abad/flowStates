---
title: "Java Collections Architecture: Stack, Queue, and Deque"
title_zh: "Java 集合架构：Stack、Queue 与 Deque"
date: 2025-11-25
author: "Dong Li"
categories: 
  - "Java Architecture"
  - "Data Structures"
  - "Best Practices"
tags:
  - "Java"
  - "Collections Framework"
  - "API Design"
  - "Legacy Code"
summary_en: "A technical analysis of why the legacy `Stack` class should be avoided in favor of the `Deque` interface. Examining the architectural decisions in the Java Collections Framework and the separation of interface and implementation."
summary_zh: "技术分析：为何应弃用传统的 `Stack` 类而改用 `Deque` 接口。探讨 Java 集合框架中的架构决策以及接口与实现的分离。"
---

[EN]
# Java Collections Architecture: The Evolution of LIFO

## 1. The Legacy Artifact: `java.util.Stack`
In the early days of Java (JDK 1.0), the Collections Framework as we know it did not exist. The `Stack` class was introduced as an extension of `Vector`.
This inheritance hierarchy—`Stack extends Vector`—is now widely considered an architectural flaw.
Because `Vector` is synchronized (thread-safe), every operation on a `Stack` incurs the overhead of acquiring a lock. In a single-threaded context, this is unnecessary performance degradation.
Furthermore, exposing random access methods (from `Vector`) violates the principle of a Stack (LIFO) abstraction.

## 2. The Modern Interface: `java.util.Deque`
Since Java 1.6, the correct way to represent a Last-In-First-Out (LIFO) stack is via the `Deque` (Double Ended Queue) interface.
The documentation explicitly states: *"A more complete and consistent set of LIFO stack operations is provided by the Deque interface and its implementations, which should be used in preference to this class."*

```java
// The Legacy Way (Avoid)
Stack<Integer> stack = new Stack<>();

// The Modern Way (Preferred)
Deque<Integer> stack = new ArrayDeque<>();
```

## 3. Implementation Choices: `ArrayDeque` vs `LinkedList`
When instantiating a `Deque`, developers typically choose between `ArrayDeque` and `LinkedList`.
*   **`ArrayDeque`**: Backed by a resizable array. It is generally faster and more memory-efficient than `LinkedList` because it avoids the overhead of node allocation and has better cache locality. It does not support `null` elements.
*   **`LinkedList`**: A doubly-linked list. It implements both `List` and `Deque`. It is useful if you need to remove elements from the interior of the collection during iteration, but for standard stack operations, `ArrayDeque` is superior.

**Conclusion**: Treat `Stack` as a historical artifact. Embrace the interface-based design of `Deque`.

[END]

[ZH]
# Java 集合架构：LIFO 的演变

## 1. 遗留产物：`java.util.Stack`
在 Java 的早期（JDK 1.0），我们熟知的集合框架（Collections Framework）尚未存在。`Stack` 类作为 `Vector` 的扩展被引入。
这种继承层次结构——`Stack extends Vector`——现在被广泛认为是一个架构缺陷。
由于 `Vector` 是同步的（线程安全），`Stack` 上的每个操作都会产生获取锁的开销。在单线程上下文中，这是不必要的性能损耗。
此外，暴露随机访问方法（来自 `Vector`）违反了栈（LIFO）抽象的原则。

## 2. 现代接口：`java.util.Deque`
自 Java 1.6 以来，表示后进先出（LIFO）栈的正确方式是通过 `Deque`（双端队列）接口。
文档明确指出：*“Deque 接口及其实现提供了一组更完整和一致的 LIFO 栈操作，应优先使用它们而非此类。”*

```java
// 传统方式（避免）
Stack<Integer> stack = new Stack<>();

// 现代方式（推荐）
Deque<Integer> stack = new ArrayDeque<>();
```

## 3. 实现选择：`ArrayDeque` vs `LinkedList`
在实例化 `Deque` 时，开发人员通常在 `ArrayDeque` 和 `LinkedList` 之间进行选择。
*   **`ArrayDeque`**：由可调整大小的数组支持。它通常比 `LinkedList` 更快且内存效率更高，因为它避免了节点分配的开销，并且具有更好的缓存局部性。它不支持 `null` 元素。
*   **`LinkedList`**：双向链表。它同时实现了 `List` 和 `Deque`。如果你需要在迭代期间从集合内部删除元素，它很有用，但对于标准的栈操作，`ArrayDeque` 更优越。

**结论**：将 `Stack` 视为历史遗留物。拥抱 `Deque` 的基于接口的设计。

[END]
