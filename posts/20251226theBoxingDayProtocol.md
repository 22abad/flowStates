---
title: "The Boxing Day Protocol: From Code Tourist to State Architect"
title_zh: "圣诞次日的算法突围：从代码游客到状态架构师"
date: 2025-12-26
author: Dong Li
categories:
  - "Algorithm Analysis"
  - "Cognitive Shift"
  - "Software Engineering"
tags:
  - "LeetCode"
  - "Backtracking"
  - "Binary Tree"
  - "Recursion"
  - "State Machine"
  - "Java"
summary_en: A comprehensive retrospective of a 12-hour coding marathon on Boxing Day. Covering the shift from backtracking snapshots (LC 113) to index arithmetic in tree construction (LC 106), and finally treating BST inorder traversal as a state machine (LC 501). A journey of 43-year-old wisdom applied to 100-day-old coding skills.
summary_zh: 记录圣诞次日长达12小时的编程马拉松。深度复盘从回溯快照（LC 113）到二叉树构造中的索引算术（LC 106），再到将 BST 中序遍历视为状态机（LC 501）的思维跃迁。这是一场用43岁的智慧去重塑100天编程技能的旅程。
---

[EN]

The Boxing Day Protocol: From Code Tourist to State Architect

![A triptych digital painting illustrating a coder's evolution from 'Tourist' to 'Architect'. Left panel: a figure follows a simple glowing path on a vast binary tree. Center panel: the figure holds a complex, luminous blueprint of the tree's structure. Right panel: the figure, now a 'State Architect', orchestrates a machine of gears and light processing the tree's data stream. The scene is unified by a warm, festive Christmas palette of gold, red, and green. Cinematic, 8K, 4:3.|800](https://assets.flowstates.me/2025/20251226boxing_day_protocol_evolution.jpg)

On Boxing Day, my biggest fear wasn't the €700 rent for my cozy room in Kingsbry, Maynooth. It was the potential loss of my 'status' and the precious 'flowstate'. But after three days of intense focus from the 24th to the 26th, a wave of relief and joy washed over me. I realized... I can do this. Haha, what a happy thought. At 100 days old in the coding world, I'm embracing the idea that "wobbling is growing." Today was not just about solving LeetCode problems; it was a "saturated rescue" mission to solidify the foundation of my algorithmic thinking.

This post documents the three phases of my cognitive shift today: The Restoration, The Construction, and The State Machine.

Phase 1: The Backtracking Contract (LeetCode 113)

The day began with Path Sum II. For months, I relied on tutorials ("Code Capriccio") as crutches. Today, I walked alone. The first obstacle was returning an empty set [] despite finding valid paths.

The breakthrough came from applying the "First Principles" of backtracking. I realized I was modifying the global state (sum) without cleaning up the crime scene.

The Mental Anchor: State Restoration

Just as path.removeLast() is required to undo a choice, sum -= node.val is the necessary counterpart. The recursion contract is strict: Leave the room exactly as you found it.

The Technical Detail: The Snapshot

In Java, objects are passed by reference. A common pitfall is adding the mutable path list directly to the result. By the time the algorithm finishes, that single whiteboard (the path) has been erased. The solution is the Snapshot:

```java
// ✅ Correct: Creates a deep copy (The Snapshot)
if (node.left == null && node.right == null && sum == targetSum) {
    res.add(new ArrayList<>(path));
}
```

This taught me to distinguish between the "Stream of Consciousness" (the mutable path) and the "Archive" (the immutable result).

Phase 2: The Architect (LeetCode 106 & 654)

Moving from traversal to construction represents a shift from "Tourist" to "Architect." Construct Binary Tree from Inorder and Postorder Traversal is the gatekeeper of this level.

My past self would have used Arrays.copyOfRange, physically slicing arrays at every step. This is low-dimensional thinking. The "Commander Mode" approach uses Index Arithmetic.

The Insight: Don't Cut the Bread, Mark It

The core challenge is calculating the size of the left subtree to determine boundaries in the postorder array. I wrestled with this specific logic for an hour, and the realization came in three parts:

1.  **Dynamic Variables**: The parameters must be dynamic recursion variables (like start), not static constants (like nums.length).
2.  **Relative Anchoring**: The leftSize calculated in the current layer is the critical baton passed to define the next layer's scope.
3.  **Limit Testing**: Boundary calculation isn't magic; it's simply handling limits or simplification (e.g., start + size - 1).

```java
// The "Golden Line" of Logic
int leftSize = rootIndex - inStart;

// Defining the boundary: Variable + leftSize = Next Layer's Limit
root.left = build(inorder, inStart, rootIndex - 1, postorder, postStart, postStart + leftSize - 1);
```

The Trap: Off-by-One Error

In Maximum Binary Tree (LC 654), I stumbled on a classic off-by-one error. My loop for (int i = start; i < end; i++) ignored the last element because end in my recursive definition was inclusive. The fix i <= end was small, but it highlighted the importance of defining precise interval invariants (e.g., left-closed, right-closed).

**The Value of "Useless" Work**

![A screenshot of LeetCode submission history, showing an initial attempt on Oct 30 and recent, successful attempts. The final two submissions show a 'Wrong Answer' followed by an 'Accepted' just two minutes later, highlighting rapid debugging.|800](https://assets.flowstates.me/2025/20251226submission_history_evolution.jpg)

This submission history tells a story. My first attempt on October 30th was, frankly, an act of "copying." I was rushing, trying to make progress without looking back. Why? Because I simply couldn't understand it. My grasp of state management and boundary control was too weak for such a task. The multiple submissions were just noise, a quantity-over-quality exercise.

But today is different. Not only can I solve it independently, but I can also debug it on my own. The proof is in the pudding: a 'Wrong Answer' followed by an 'Accepted' solution just two minutes later. This highlights two core truths I've come to believe:

1.  **Just keep going, and you will see.** The path forward clarifies itself through persistence.
2.  **What seems meaningless is, in fact, deeply meaningful.** This accumulation is a quantitative change that is impossible to measure until the moment of qualitative breakthrough. Only then do you realize the profound impact of all that "useless work."

Phase 3: The State Machine (LeetCode 98, 530, 501)

The final boss was the Binary Search Tree (BST). The "High Dimensional" insight here is simple but profound: BST Inorder Traversal = Sorted Stream.

The Invisible Wall: Integer Boundaries

In Validate BST (LC 98), I faced the edge case of Integer.MIN_VALUE. Initializing min = Integer.MIN_VALUE causes a bug when the root itself is that minimum value. The fix required upgrading the "guards" (boundary parameters) to Long or using null to represent infinity.

The Clerk: BST as a State Machine

For Find Mode (LC 501), I initially feared the complexity. However, by treating the inorder traversal as a stream, I converted the problem into a State Machine. I became a "Clerk" holding a ledger (prev pointer).

The protocol was pure logic:

1.  **Compare**: Is current == prev? Increment count.
2.  **Challenge**: Does count > maxCount? Clear the list, update the king.
3.  **Record**: prev = current.

Conclusion: The Art of Ignore

Today proved that efficiency isn't just about speed; it's about depth. I spent 12 hours on 6 problems, but I internalized concepts that usually take weeks. As an "Old Urchin," I accept the wobbling. I ignore the noise of "fast solutions" to pursue the structural joy of true understanding.

[END]

[ZH]

圣诞次日的算法突围：从代码游客到状态架构师

![一幅三联画风格的数字艺术作品，描绘了一位程序员从“游客”到“建筑师”的认知进化。左边部分：一个身影沿着一棵巨大二叉树上的简单发光路径前行。中间部分：这个身影手持一张复杂的、散发着光芒的树结构蓝图。右边部分：身影已成为“状态架构师”，正在指挥一个由齿轮和光组成的机器，处理着树的数据流。整个画面由温暖的金色、红色和绿色的节日色调统一起来。电影感，8K，4:3。|800](https://assets.flowstates.me/2025/20251226boxing_day_protocol_evolution.jpg)

圣诞次日，我最担心的不是那间 700 欧租来的卧室是否温暖舒适，而是我是否会失去来之不易的“身份”（status）和宝贵的心流（flowstate）。但经过从 24 号到 26 号这三天的闭关，我发现，我好像可以掌控这一切。哈哈，真开心！作为编程世界里一个只有 100 天大的“婴儿”，我深知“摇晃即成长”（Wobbling is growing）。今天不仅仅是做几道 LeetCode 题，这是一场“饱和式救援”，旨在通过高强度的训练夯实我的算法地基。

这篇文章记录了我今天经历的三个认知跃迁阶段：恢复、构造与状态机。

第一阶段：回溯的契约（LeetCode 113）

这一天始于 路径总和 II (Path Sum II)。几个月来，我一直依赖教程（如“代码随想录”）作为拐杖。今天，我选择独自前行。遇到的第一个障碍是：明明找到了路径，程序却返回了空集 []。

突破来自于对回溯“第一性原理”的应用。我意识到我一直在修改全局状态（sum），却从未清理“案发现场”。

心理锚点：状态恢复

就像 path.removeLast() 是撤销选择的必选项一样，sum -= node.val 是它逻辑上的孪生兄弟。递归的契约非常严格：离开房间时，必须把它恢复成你进来时的样子。

技术细节：快照机制

在 Java 中，对象是引用传递的。一个常见的陷阱是直接把可变的 path 列表加入结果集。当算法结束时，那块唯一的“白板”（path）已经被擦得干干净净。解决方案是快照 (Snapshot)：

```java
// ✅ 正确：创建深拷贝（快照）
if (node.left == null && node.right == null && sum == targetSum) {
    res.add(new ArrayList<>(path));
}
```

这教会了我区分“意识流”（可变的 path）和“档案馆”（不可变的结果）。

第二阶段：建筑师（LeetCode 106 & 654）

从“遍历”到“构造”代表了从“游客”到“建筑师”的转变。从中序与后序遍历序列构造二叉树 是这一关的守门人。

过去的我可能会使用 Arrays.copyOfRange，在每一步递归中物理切割数组。这是低维思维。“指挥官模式”使用的是索引算术 (Index Arithmetic)。

顿悟：不要切面包，做标记

核心挑战是计算左子树的大小，从而确定后序数组中的边界。其实这一行代码我思考了整整一个小时，我的心得是：

1.  **变量必须是变量**：这里面的变量必须是递归中的动态指针（如 start），绝不能是 nums.length 这种静态常量。
2.  **层级传递**：变量配合当前递归层计算出的 leftSize，共同决定了下层递归的传入参数。
3.  **边界即极限**：计算边界时，与其死记硬背，不如考虑极限情况或进行简化处理（例如 start + size - 1）。

```java
// 逻辑的“黄金分割线”
int leftSize = rootIndex - inStart;
// 边界计算：变量 + leftSize = 下一层的极限
root.left = build(inorder, inStart, rootIndex - 1, postorder, postStart, postStart + leftSize - 1);
```

陷阱：差一错误 (Off-by-One Error)

在 最大二叉树 (LC 654) 中，我被一个经典的差一错误绊倒了。我的循环 for (int i = start; i < end; i++) 忽略了最后一个元素，因为我在递归定义中 end 是包含在内的（闭区间）。修正为 i <= end 虽然只是一个小改动，但它强调了定义精确区间不变量的重要性。

**“无用功”的价值**

![一张 LeetCode 提交历史截图，显示了10月30日的初次尝试和近期的成功尝试。最后两次提交记录为一个“错误答案”，紧接着在两分钟后就是一个“已接受”，凸显了快速调试的能力。|800](https://assets.flowstates.me/2025/20251226submission_history_evolution.jpg)

这张提交记录本身就在讲一个故事。10 月 30 日的第一次提交，说白了就是“抄”。我当时在疯狂地“赶进度”，根本没回头看。原因？因为根本看不懂。以我当时对状态的理解和边界的控制能力来说，那是不可能独立完成的任务，后续的多次提交也仅仅是凑数量而已。

但如今，我不仅可以独立做出来，甚至可以独立 debug。证据就在那里：一个红色的 "Wrong Answer" 之后，仅仅两分钟就迎来了正式的 "Accepted"。这恰恰说明了两个道理：

1.  **只管走下去，你终将看到风景 (Just keep going, and you will see)。** 坚持本身就会让道路变得清晰。
2.  **看似无意义的量变，实则充满了意义。** 这种积累是无法被量化的，直到质变发生的那一刻，你才会突然意识到所有那些“无用功”的真正作用。

第三阶段：状态机（LeetCode 98, 530, 501）

最终的 BOSS 是二叉搜索树（BST）。这里的高维洞见简单而深刻：BST 中序遍历 = 有序流。

隐形墙：整数边界

在 验证二叉搜索树 (LC 98) 中，我遭遇了 Integer.MIN_VALUE 的边界情况。初始化 min = Integer.MIN_VALUE 会导致当根节点本身就是该最小值时出现 Bug。修正方案是将“保镖”（边界参数）升级为 Long，或者使用 null 来代表无穷大。

书记员：作为状态机的 BST

对于 二叉搜索树中的众数 (LC 501)，起初我通过代码长度判断它“超出能力范围”。然而，通过将中序遍历视为数据流，我把问题转化为了一个状态机。我变成了一个拿着账本（prev 指针）的“书记员”。

核心协议纯粹是逻辑：

1.  **比对**：current == prev 吗？计数器加一。
2.  **挑战**：count > maxCount 吗？清空列表，更新王者。
3.  **记录**：prev = current。

结论：无视的艺术 (The Art of Ignore)

今天证明了，效率不仅仅关乎速度，更关乎深度。我在 6 道题上花了 12 个小时，但我内化了通常需要数周才能掌握的概念。作为一个“老顽童”，我接受这种摇晃。我无视“快速解题”的噪音，去追求真正理解结构的快乐。

[END]
