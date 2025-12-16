---
title: "Algorithmic Archetypes: A Taxonomy of Binary Tree Patterns"
title_zh: "算法原型：二叉树模式分类学"
date: 2025-11-29
author: "Dong Li"
categories:
  - "Algorithm Analysis"
  - "Pattern Recognition"
tags:
  - "Binary Tree"
  - "Taxonomy"
  - "LeetCode"
summary_en: "A comprehensive taxonomy of Binary Tree logic, categorizing problems into five archetypes: Traversal, Construction, LCA, Validation, and Modification. Providing a structured approach to mastering tree algorithms via pattern recognition."
summary_zh: "二叉树逻辑的综合分类学，将问题归类为五个原型：遍历、构造、LCA、验证和修改。提供一种通过模式识别掌握树算法的结构化方法。"
---

[EN]
![Binary Tree Strategy|600](https://assets.flowstates.me/2025/20251128binaryTree.jpg)

# Pattern Recognition Strategy

Efficient review necessitates prioritization. Rather than exhaustive iteration, we focus on archetypes.
Mastering the following 5 core archetypes and their implementation variations (Recursion vs. Iteration) provides a comprehensive coverage of Binary Tree logic.

## Archetype 1: Traversal Logic (Pre/In/Post)

- **Reference**: [LeetCode 94 (In)](https://leetcode.com/problems/binary-tree-inorder-traversal/) / [144 (Pre)](https://leetcode.com/problems/binary-tree-preorder-traversal/) / [145 (Post)](https://leetcode.com/problems/binary-tree-postorder-traversal/)
- **Objective**: Mastery of Recursion, Standard Iteration, and Unified Iteration.

### Implementation 1: Recursion (The Standard)

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        dfs(root, res);
        return res;
    }

    private void dfs(TreeNode node, List<Integer> res) {
        if (node == null) return;
        // Pre-order position
        dfs(node.left, res);
        res.add(node.val); // In-order position
        dfs(node.right, res);
        // Post-order position
    }
}
```

### Implementation 2: Standard Iteration (Explicit Stack)

_Simulating the call stack. Note the "Left-Road-First" logic._

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode curr = root;

        while (curr != null || !stack.isEmpty()) {
            // 1. Drill down to the leftmost node
            if (curr != null) {
                stack.push(curr);
                curr = curr.left;
            }
            // 2. Hit bottom, pop and process
            else {
                curr = stack.pop();
                res.add(curr.val); // In-order processing
                curr = curr.right; // Turn right
            }
        }
        return res;
    }
}
```

### Implementation 3: Unified Iteration (Marker Method)

_The robust template. Using `null` as a state marker to unify logic across all traversal orders._

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        if (root != null) stack.push(root);

        while (!stack.isEmpty()) {
            TreeNode node = stack.peek();
            if (node != null) {
                stack.pop(); // Pop to re-add in correct order

                // Order for In-Order: Right -> Center -> Left (Stack is LIFO)
                if (node.right != null) stack.push(node.right);  // Right
                stack.push(node);                                // Center
                stack.push(null);                                // ⚡️ MARKER (Visit later)
                if (node.left != null) stack.push(node.left);    // Left
            } else {
                stack.pop(); // Pop null
                node = stack.pop(); // Pop actual node
                res.add(node.val); // Process
            }
        }
        return res;
    }
}
```
[END]

[ZH]
![Binary Tree Strategy|600](https://assets.flowstates.me/2025/20251128binaryTree.jpg)

# 模式识别策略

高效复习需要优先级。我们不进行穷举迭代，而是关注原型。
掌握以下 5 个核心原型及其实现变体（递归 vs. 迭代）提供了对二叉树逻辑的全面覆盖。

## 原型 1：遍历逻辑 (前/中/后)

- **参考**：[LeetCode 94 (中)](https://leetcode.com/problems/binary-tree-inorder-traversal/) / [144 (前)](https://leetcode.com/problems/binary-tree-preorder-traversal/) / [145 (后)](https://leetcode.com/problems/binary-tree-postorder-traversal/)
- **目标**：掌握递归、标准迭代和统一迭代。

### 实现 1：递归（标准）

```java
// 见上文代码块
```

### 实现 2：标准迭代（显式栈）

_模拟调用栈。注意“左路优先”逻辑。_

```java
// 见上文代码块
```

### 实现 3：统一迭代（标记法）

_鲁棒的模板。使用 `null` 作为状态标记，以统一所有遍历顺序的逻辑。_

```java
// 见上文代码块
```
[END]
