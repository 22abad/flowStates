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
summary_en: "A comprehensive guide to 4 foundational Binary Tree problems. Includes Recursive, Iterative, and Unified solutions for each. Mastering these patterns allows you to solve almost any tree problem."
summary_zh: "二叉树 4 道母题的终极全解指南。每道题包含递归、迭代、统一迭代等多种解法。吃透这些模式，足以应对绝大多数二叉树问题。"
---

![Binary Tree Strategy|600](https://assets.flowstates.me/2025/20251128binaryTree.jpg)

[EN]
# The Strategy: Why These 4?

We don't have time to re-do all 30 problems. We need to hit the **Vital Points**.
Binary Trees essentially test only two things:
1.  **Traversal Order**: Can you visit nodes in the exact order you want? (Recursion is easy; Iteration is the test).
2.  **Structure Manipulation**: Can you change links or build nodes based on logic?

If you master the following 4 problems and their variations, you master the Tree.
[END]

[ZH]
# 战略：为什么选这 4 道？

我们没时间把那 30 道题重刷一遍。我们要打**七寸**。
二叉树本质上只考两件事：
1.  **遍历顺序**：你能按你想要的顺序访问节点吗？（递归很简单，迭代才是试金石）。
2.  **结构操作**：你能根据逻辑修改指针或构建节点吗？

搞定下面这 4 道题及其变种，二叉树你就通关了。
[END]

---

[EN]
## 1. The Foundation: Traversal (Pre/In/Post)
* **Problem**: LeetCode 94 / 144 / 145.
* **Goal**: Master both Recursion and Iteration.
[END]

[ZH]
## 1. 地基：遍历（前/中/后序）
* **对应题目**: LeetCode 94 / 144 / 145。
* **目标**: 同时掌握递归和迭代。
[END]

### Solution 1: Recursion (The Basic) / 递归法（基础）

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        dfs(root, res);
        return res;
    }
    
    private void dfs(TreeNode node, List<Integer> res) {
        if (node == null) return;
        // Pre-order: res.add(node.val);
        dfs(node.left, res);
        res.add(node.val); // In-order position
        dfs(node.right, res);
        // Post-order: res.add(node.val);
    }
}
```
### Solution 2: Standard Iteration (Stack) / 标准迭代法（栈）

[ZH] 这是最直观的迭代法，模拟递归栈。前序最简单，中序需要指针辅助，后序最难（通常用双栈法或反转前序法）。这里展示**中序**的标准迭代。 [END]



```Java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>(); // Deque<TreeNode> stack = new ArrayDeque<>();
        TreeNode curr = root;
        
        while (curr != null || !stack.isEmpty()) {
            // 1. Reach the leftmost node
            if (curr != null) {
                stack.push(curr);
                curr = curr.left; // Go left
            } 
            // 2. Process the node
            else {
                curr = stack.pop();
                res.add(curr.val); // Record (Center)
                curr = curr.right; // Go right
            }
        }
        return res;
    }
}
```

### Solution 3: Unified Iteration (Marker Method) / 统一迭代法（标记法）

[ZH] 这是**最强通用模板**。只需要调整入栈顺序，就能通吃前中后序，逻辑完全一致。 核心思想：用 `null` 标记“来过但未处理”的节点。 [END]


```Java
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
                // Current: In-order (Left -> Center -> Right)
                // Stack Order (Reverse): Right -> Center -> Left
                
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
```

---

[EN]

## 2. The Construction: Build Tree (Pre + In)

- **Problem**: LeetCode 105.
    
- **Why**: The ultimate test of **Index Manipulation** and **Recursion**. You must split arrays into "Left Subtree" and "Right Subtree" precisely. [END]
    

[ZH]

## 2. 构造：从前序与中序构造二叉树

- **对应题目**: LeetCode 105。
    
- **理由**: 这是对 **数组下标** 和 **递归拆解** 的终极考验。你必须精准地计算出左子树和右子树在数组中的范围。 [END]
    


```Java
class Solution {
    // Map speeds up finding index in inorder array (O(1))
    Map<Integer, Integer> map = new HashMap<>();

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        for (int i = 0; i < inorder.length; i++) {
            map.put(inorder[i], i);
        }
        // Range: Left Inclusive, Right Inclusive
        return helper(preorder, 0, preorder.length - 1, inorder, 0, inorder.length - 1);
    }

    private TreeNode helper(int[] preorder, int preStart, int preEnd, 
                            int[] inorder, int inStart, int inEnd) {
        // Base Case: Empty array range
        if (preStart > preEnd) return null;

        // 1. Find Root Value (First of Preorder)
        int rootVal = preorder[preStart];
        TreeNode root = new TreeNode(rootVal);

        // 2. Find Root Index in Inorder
        int index = map.get(rootVal);
        
        // 3. Calculate Left Subtree Size (Crucial!)
        int leftSize = index - inStart;

        // 4. Recursion
        // Left Child:
        // Pre: [preStart+1, preStart+leftSize]
        // In:  [inStart, index-1]
        root.left = helper(preorder, preStart + 1, preStart + leftSize, inorder, inStart, index - 1);
        
        // Right Child:
        // Pre: [preStart+leftSize+1, preEnd]
        // In:  [index+1, inEnd]
        root.right = helper(preorder, preStart + leftSize + 1, preEnd, inorder, index + 1, inEnd);

        return root;
    }
}
```

---

[EN]

## 3. The Logic: Lowest Common Ancestor (LCA)

- **Problem**: LeetCode 236.
    
- **Why**: The pinnacle of **Post-order Traversal** (Bottom-up). You need information from children to make a decision at the parent. [END]
    

[ZH]

## 3. 逻辑：最近公共祖先 (LCA)

- **对应题目**: LeetCode 236。
    
- **理由**: 这是 **后序遍历 (Bottom-up)** 的巅峰。你需要收集左右孩子的返回值，然后在父节点做决策。逻辑极其精简。 [END]
    



```Java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // 1. Termination
        // Found p or q, or hit bottom -> return self
        if (root == null || root == p || root == q) return root;

        // 2. Recursion (Go verify left and right subtrees)
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);

        // 3. Logic (Backtracking / Post-order processing)
        
        // If both sides returned non-null, it means p is on one side and q is on the other.
        // Current node is the LCA!
        if (left != null && right != null) return root; 
        
        // If only one side returned something, pass it up.
        if (left != null) return left;   
        if (right != null) return right; 
        
        // Nothing found
        return null;
    }
}
```

---

[EN]

## 4. The Operation: Delete Node in BST

- **Problem**: LeetCode 450.
    
- **Why**: The comprehensive exam for BST. It combines **Search** and **Pointer Manipulation**. The tricky part is handling a node with two children. [END]
    

[ZH]

## 4. 操作：删除 BST 中的节点

- **对应题目**: LeetCode 450。
    
- **理由**: BST 操作的综合大题。结合了 **搜索** 和 **指针操作**。最难的是处理“双子节点”的情况——需要找到右子树的最小值来“继位”。 [END]
    

### Solution 1: Recursive (Standard) / 递归法（标准）



```Java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null) return null;

        // 1. Search
        if (root.val > key) {
            root.left = deleteNode(root.left, key);
            return root;
        } else if (root.val < key) {
            root.right = deleteNode(root.right, key);
            return root;
        } else {
            // 2. Found: Delete Logic
            
            // Case A: Leaf or One Child (Directly return the other child)
            if (root.left == null) return root.right;
            if (root.right == null) return root.left;

            // Case B: Two Children
            // Strategy: Find the MIN node in the RIGHT subtree (Successor)
            TreeNode minNode = root.right;
            while (minNode.left != null) minNode = minNode.left;
            
            // Trick: Move root's left subtree to minNode's left
            // (This changes tree structure but keeps BST property)
            minNode.left = root.left; 
            return root.right; // Replace root with right child
        }
    }
}
```

### Solution 2: Iterative (Hardcore) / 迭代法（硬核）

[ZH] 迭代法删除 BST 节点非常繁琐，因为你需要维护 `parent` 指针。面试中通常写递归即可，但了解迭代逻辑有助于理解指针操作。 [END]



```Java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null) return null;
        
        TreeNode cur = root;
        TreeNode pre = null; // Parent node
        
        // 1. Find the node
        while (cur != null) {
            if (cur.val == key) break;
            pre = cur;
            if (cur.val > key) cur = cur.left;
            else cur = cur.right;
        }
        if (cur == null) return root; // Not found

        // 2. Delete logic
        // If root is the target
        if (pre == null) return deleteOneNode(cur);
        
        // If target is left child
        if (pre.left != null && pre.left.val == key) {
            pre.left = deleteOneNode(cur);
        }
        // If target is right child
        if (pre.right != null && pre.right.val == key) {
            pre.right = deleteOneNode(cur);
        }
        return root;
    }

    // Helper: Delete a specific node and rearrange children
    private TreeNode deleteOneNode(TreeNode target) {
        if (target == null) return null;
        if (target.right == null) return target.left;
        
        // Put target.left to the leftmost of target.right
        TreeNode cur = target.right;
        while (cur.left != null) cur = cur.left;
        cur.left = target.left;
        
        return target.right;
    }
}
```

