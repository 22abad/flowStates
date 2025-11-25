Markdown---
title: "Java Collections Demystified: Stack, Queue, Deque, and Why 'Stack' is Dead"
title_zh: "Java 集合框架解惑：Stack, Queue, Deque 以及为什么 Stack 已死"
date: 2025-11-25
author: "Dong"
categories: 
  - "CS"
  - "Java"
tags:
  - "Data Structure"
  - "Best Practice"
  - "Interview Prep"
summary_en: "Clearing up the confusion between Stack, Deque, and LinkedList in Java. Why you should stop using the `Stack` class, how to use `Deque` as a stack, and the difference between Interface (Identity) and Implementation (Body)."
summary_zh: "彻底理清 Java 中 Stack, Deque 和 LinkedList 的纠葛。为什么你应该停止使用 `Stack` 类，如何正确地用 `Deque` 实现栈，以及接口（身份）与实现（肉体）的区别。"
---

[EN]
# Java Collections Demystified

## 1. The Confusion: `Stack` vs `LinkedList`
I recently stumbled upon a common compilation error that confuses many Java beginners (including me):

```java
// ❌ Compile Error: Incompatible types
Stack<TreeNode> stack = new LinkedList<>(); 
Why does this fail? Isn't LinkedList capable of being a stack? Yes.But in Java's type system:Stack is a specific Class (a very old one).LinkedList is another specific Class.They have no inheritance relationship. A LinkedList IS NOT A Stack.It's like trying to say: CassettePlayer player = new MP3Player();. They both play music, but they are fundamentally different machines.[END][ZH]Java 集合框架解惑1. 困惑源头：Stack vs LinkedList最近我遇到了一个让很多 Java 初学者（包括我）摸不着头脑的编译错误：Java// ❌ 编译错误：类型不兼容
Stack<TreeNode> stack = new LinkedList<>(); 
为什么会报错？LinkedList 难道不能当栈用吗？当然可以。但在 Java 的类型系统中：Stack 是一个具体的 类（而且是个老古董）。LinkedList 是另一个具体的 类。它们之间没有继承关系。LinkedList 不是 Stack。这就像是写代码说：老式磁带机 player = new MP3播放器();。虽然它们都能放歌，但它们本质上是两种不同的机器。[END][EN]2. The Solution: Interface vs ImplementationTo fix this, we need to separate Identity (Interface) from Body (Implementation).If you want the behavior of a Stack (LIFO - Last In First Out), you should use the modern interface Deque (Double Ended Queue).Correct Ways to Declare a Stack in Java:Java// Option A: Array-based (Fastest, Recommended)
Deque<Integer> stack = new ArrayDeque<>();

// Option B: Linked-List-based (If you need node manipulation)
Deque<Integer> stack = new LinkedList<>();
Here, Deque is the Contract (Identity), and ArrayDeque / LinkedList is the Worker (Body).[END][ZH]2. 解决方案：接口 vs 实现要解决这个问题，我们需要把 身份（接口） 和 肉体（实现） 分开。如果你想要一个栈的 行为（后进先出），你应该使用现代的标准接口 Deque（双端队列）。Java 中声明栈的正确姿势：Java// 方案 A：基于数组（最快，推荐）
Deque<Integer> stack = new ArrayDeque<>();

// 方案 B：基于链表（如果你需要节点操作）
Deque<Integer> stack = new LinkedList<>();
在这里，Deque 是 合同（身份），而 ArrayDeque / LinkedList 是真正干活的 工人（肉体）。[END][EN]3. Why is the Stack Class Dead?You might ask, why not just use Stack<Integer> s = new Stack<>();?The Stack class is a relic from JDK 1.0.It inherits from Vector, which means all its methods are synchronized.This makes it thread-safe but slow for single-threaded tasks (like LeetCode algorithms).Even the official Java documentation recommends using Deque instead.Summary Table:GoalInterface (Left)Implementation (Right)Dynamic ArrayListArrayListLinked ListListLinkedListStack (LIFO)DequeArrayDequeQueue (FIFO)QueueLinkedList / ArrayDequeStop using Stack. Embrace Deque.[END][ZH]3. 为什么 Stack 类已死？你可能会问，为什么不直接用 Stack<Integer> s = new Stack<>(); 呢？Stack 类是 JDK 1.0 时代的遗物。它继承自 Vector，这意味着它所有的方法都是 synchronized（同步的）。这让它虽然线程安全，但在单线程任务（比如刷算法题）中非常慢。就连 Java 官方文档都建议改用 Deque。总结表：目标功能接口 (左边)实现类 (右边)动态数组ListArrayList链表ListLinkedList栈 (后进先出)DequeArrayDeque队列 (先进先出)QueueLinkedList / ArrayDeque别再用 Stack 了。拥抱 Deque 吧。[END]