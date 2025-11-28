---
title: "Binary Tree Elite Squad: The Only 4 Problems You Need for Mastery (Comprehensive Edition)"
title_zh: "二叉树特种兵：二刷只需搞定这 4 道母题（全解版）"
date: 2025-11-29
author: "Dong"
categories: 
  - "Algorithm"
  - "Binary Tree"
tags:
  - "High Efficiency"
  - "Patterns"
  - "LeetCode"
  - "Java"
summary_en: "A strategic selection of 4 Binary Tree problems for the second pass. Mastering these covers Traversal (Iterative), Construction, LCA, and BST Modification. Quality over quantity. Deep understanding over rote memorization."
summary_zh: "为二刷精心挑选的 4 道二叉树母题。精通这些将覆盖迭代遍历、构造、最近公共祖先和 BST 修改。重质量轻数量，重深度理解轻死记硬背。"
---

[EN]
![Binary Tree Strategy|600](https://assets.flowstates.me/2025/20251128binaryTree.jpg)

# The Strategy: Why These 4?

We don't have time to re-do all 30 problems. We need to hit the **Vital Points**.
Binary Trees essentially test only two things:
1.  **Traversal Order**: Can you visit nodes in the exact order you want? (Recursion is easy; Iteration is the test).
2.  **Structure Manipulation**: Can you change links or build nodes based on logic?

If you master the following 4 problems, you master the Tree.

## 1. The Foundation: Unified Iterative Traversal
* **Problem**: Equivalent to LeetCode 94 / 144 / 145.
* **Why**: Recursion is an implicit stack; Iteration is an explicit stack. The **Unified Style** (Marker Method) lets you control Pre/In/Post order by simply swapping lines.

```java
// Unified Iterative Method (Marker Style)
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        if (root != null) stack.push(root);
        
        while (!stack.isEmpty()) {
            TreeNode node = stack.peek();
            if (node != null) {
                stack.pop(); // Pop to re-add in correct order
                
                // Adjust these lines for Pre/In/Post Order
                if (node.right != null) stack.push(node.right);  // Right
                stack.push(node);                                // Center
                stack.push(null);                                // ⚡️ MARKER (Not visited yet)
                if (node.left != null) stack.push(node.left);    // Left
                
            } else {
                stack.pop();        // Pop the null marker
                node = stack.pop(); // Pop the real node
                res.add(node.val);  // Process
            }
        }
        return res;
    }
}
````

## 2\. The Construction: Build Tree (Pre + In)

  * **Problem**: LeetCode 105.
  * **Why**: The ultimate test of **Index Manipulation** and **Recursion**. You must split arrays into "Left Subtree" and "Right Subtree" precisely.

<!-- end list -->

```java
class Solution {
    Map<Integer, Integer> map = new HashMap<>();

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        for (int i = 0; i < inorder.length; i++) {
            map.put(inorder[i], i);
        }
        return helper(preorder, 0, preorder.length - 1, inorder, 0, inorder.length - 1);
    }

    private TreeNode helper(int[] preorder, int preStart, int preEnd, 
                            int[] inorder, int inStart, int inEnd) {
        if (preStart > preEnd) return null;

        int rootVal = preorder[preStart];
        TreeNode root = new TreeNode(rootVal);

        int index = map.get(rootVal);
        int leftSize = index - inStart; // ⚡️ Critical: Size of left subtree

        root.left = helper(preorder, preStart + 1, preStart + leftSize, inorder, inStart, index - 1);
        root.right = helper(preorder, preStart + leftSize + 1, preEnd, inorder, index + 1, inEnd);

        return root;
    }
}
```

## 3\. The Logic: Lowest Common Ancestor (LCA)

  * **Problem**: LeetCode 236.
  * **Why**: The pinnacle of **Post-order Traversal** (Bottom-up). You need information from children to make a decision at the parent.

<!-- end list -->

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // 1. Termination
        if (root == null || root == p || root == q) return root;

        // 2. Recursion
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);

        // 3. Logic (Backtracking)
        if (left != null && right != null) return root; // Found split point
        if (left != null) return left;   // Pass up result from left
        if (right != null) return right; // Pass up result from right
        return null;
    }
}
```

## 4\. The Operation: Delete Node in BST

  * **Problem**: LeetCode 450.
  * **Why**: The comprehensive exam for BST. It combines **Search** and **Pointer Manipulation**. The tricky part is handling a node with two children.

<!-- end list -->

```java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null) return null;

        if (root.val > key) {
            root.left = deleteNode(root.left, key);
            return root;
        } else if (root.val < key) {
            root.right = deleteNode(root.right, key);
            return root;
        } else {
            // Found it. Delete logic.
            if (root.left == null) return root.right;
            if (root.right == null) return root.left;

            // Two children: Find successor (Min node in Right Subtree)
            TreeNode minNode = root.right;
            while (minNode.left != null) minNode = minNode.left;
            
            // Grafting
            minNode.left = root.left; 
            return root.right; 
        }
    }
}
```

[END]

[ZH]

# 战略：为什么选这 4 道？

我们没时间把那 30 道题重刷一遍。我们要打**七寸**。
二叉树本质上只考两件事：

1.  **遍历顺序**：你能按你想要的顺序访问节点吗？（递归很简单，迭代才是试金石）。
2.  **结构操作**：你能根据逻辑修改指针或构建节点吗？

搞定下面这 4 道，二叉树你就通关了。

## 1\. 地基：统一迭代法遍历

  * **对应题目**: LeetCode 94/144/145。
  * **理由**: 递归是隐式栈，迭代是显式栈。**统一迭代法（标记法）** 让你只需要交换代码行顺序就能实现前、中、后序遍历，是真正的万能钥匙。

<!-- end list -->

```java
// 统一迭代法 (标记法)
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        if (root != null) stack.push(root);
        
        while (!stack.isEmpty()) {
            TreeNode node = stack.peek();
            if (node != null) {
                stack.pop(); // 弹出，为了重新按顺序压栈
                
                // 调整这三行的顺序即可改变遍历方式 (当前是中序: 左-中-右 -> 压栈顺序: 右-中-左)
                if (node.right != null) stack.push(node.right);  // 右
                stack.push(node);                                // 中
                stack.push(null);                                // ⚡️ 标记 (来过，未处理)
                if (node.left != null) stack.push(node.left);    // 左
                
            } else {
                stack.pop();        // 弹出标记 null
                node = stack.pop(); // 弹出真正的数据节点
                res.add(node.val);  // 处理
            }
        }
        return res;
    }
}
```

## 2\. 构造：从前序与中序构造二叉树

  * **对应题目**: LeetCode 105。
  * **理由**: 这是对 **数组下标** 和 **递归拆解** 的终极考验。你必须精准地计算出左子树和右子树在数组中的范围。

<!-- end list -->

```java
class Solution {
    // Map 加速查找根节点下标 (O(1))
    Map<Integer, Integer> map = new HashMap<>();

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        for (int i = 0; i < inorder.length; i++) {
            map.put(inorder[i], i);
        }
        // 区间：左闭右闭
        return helper(preorder, 0, preorder.length - 1, inorder, 0, inorder.length - 1);
    }

    private TreeNode helper(int[] preorder, int preStart, int preEnd, 
                            int[] inorder, int inStart, int inEnd) {
        // 终止条件：数组为空
        if (preStart > preEnd) return null;

        // 1. 找到根节点值 (前序第一个)
        int rootVal = preorder[preStart];
        TreeNode root = new TreeNode(rootVal);

        // 2. 找到根在中序中的位置
        int index = map.get(rootVal);
        
        // 3. 计算左子树大小 (关键!)
        int leftSize = index - inStart;

        // 4. 递归构造
        // 左子树:
        // 前序: [preStart+1, preStart+leftSize]
        // 中序: [inStart, index-1]
        root.left = helper(preorder, preStart + 1, preStart + leftSize, inorder, inStart, index - 1);
        
        // 右子树:
        // 前序: [preStart+leftSize+1, preEnd]
        // 中序: [index+1, inEnd]
        root.right = helper(preorder, preStart + leftSize + 1, preEnd, inorder, index + 1, inEnd);

        return root;
    }
}
```

## 3\. 逻辑：最近公共祖先 (LCA)

  * **对应题目**: LeetCode 236。
  * **理由**: 这是 **后序遍历 (Bottom-up)** 的巅峰。你需要收集左右孩子的返回值，然后在父节点做决策。逻辑极其精简。

<!-- end list -->

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // 1. 终止条件
        // 到底了，或者找到目标了 -> 返回自己
        if (root == null || root == p || root == q) return root;

        // 2. 递归 (去左右子树找)
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);

        // 3. 逻辑判断 (回溯/后序处理)
        
        // 左右各找到一个，说明 p 和 q 分居两侧 -> 当前节点就是 LCA
        if (left != null && right != null) return root; 
        
        // 只有一边找到了，说明 LCA 在那一边，往上传
        if (left != null) return left;   
        if (right != null) return right; 
        
        // 啥也没找到
        return null;
    }
}
```

## 4\. 操作：删除 BST 中的节点

  * **对应题目**: LeetCode 450。
  * **理由**: BST 操作的综合大题。结合了 **搜索** 和 **指针操作**。最难的是处理“双子节点”的情况——需要找到右子树的最小值来“继位”。

<!-- end list -->

```java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null) return null;

        // 1. 搜索
        if (root.val > key) {
            root.left = deleteNode(root.left, key);
            return root;
        } else if (root.val < key) {
            root.right = deleteNode(root.right, key);
            return root;
        } else {
            // 2. 找到了：删除逻辑
            
            // 情况 A: 叶子或单孩子 (直接让孩子继位)
            if (root.left == null) return root.right;
            if (root.right == null) return root.left;

            // 情况 B: 双孩子
            // 策略: 找到右子树中的最小值 (继承人)
            TreeNode minNode = root.right;
            while (minNode.left != null) minNode = minNode.left;
            
            // 技巧: 把root的左子树，嫁接到继承人的左边
            // (这改变了树结构，但保持了BST性质，代码最少)
            minNode.left = root.left; 
            return root.right; // 右孩子上位
        }
    }
}
```

[END]

```
```