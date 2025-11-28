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

If you master the following 4 problems and their variations, you master the Tree.
[END]

[ZH]
![二叉树战略图|600](https://assets.flowstates.me/2025/20251128binaryTree.jpg)

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
* **Problem**: [LeetCode 94 (In)](https://leetcode.cn/problems/binary-tree-inorder-traversal/) / [144 (Pre)](https://leetcode.cn/problems/binary-tree-preorder-traversal/) / [145 (Post)](https://leetcode.cn/problems/binary-tree-postorder-traversal/)
* **Goal**: Master both Recursion and Iteration. The **Unified Style** (Marker Method) is a must-know pattern.

### Solution 1: Recursion (The Basic)

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
````

### Solution 2: Standard Iteration (Stack)

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
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

### Solution 3: Unified Iteration (Marker Method)

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
```

[END]

[ZH]

## 1\. 地基：遍历（前/中/后序）

  * **对应题目**: [LeetCode 94 (中)](https://leetcode.cn/problems/binary-tree-inorder-traversal/) / [144 (前)](https://leetcode.cn/problems/binary-tree-preorder-traversal/) / [145 (后)](https://leetcode.cn/problems/binary-tree-postorder-traversal/)
  * **目标**: 同时掌握递归和迭代。重点掌握**统一迭代法（标记法）**，它是解决所有遍历问题的万能钥匙。

### 解法 1：递归法（基础）

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

### 解法 2：标准迭代法（栈）

*思路：一路向左钻到底，撞墙了就回头处理，然后转向右边。*

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode curr = root;
        
        while (curr != null || !stack.isEmpty()) {
            // 1. Reach the leftmost node (一路向左)
            if (curr != null) {
                stack.push(curr);
                curr = curr.left; 
            } 
            // 2. Process the node (到头了，弹出处理)
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

### 解法 3：统一迭代法（标记法）

*思路：用 null 标记“来过但未处理”的节点。只需交换 push 顺序即可实现前中后序。*

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

[END]

-----

[EN]

## 2\. The Construction: Build Tree (Pre + In)

  * **Problem**: [LeetCode 105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
  * **Why**: The ultimate test of **Index Manipulation** and **Recursion**. You must split arrays into "Left Subtree" and "Right Subtree" precisely.

<!-- end list -->

```java
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

[END]

[ZH]

## 2\. 构造：从前序与中序构造二叉树

  * **对应题目**: [LeetCode 105. 从前序与中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
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
        // Base Case: Empty array range
        if (preStart > preEnd) return null;

        // 1. Find Root Value (First of Preorder)
        int rootVal = preorder[preStart];
        TreeNode root = new TreeNode(rootVal);

        // 2. Find Root Index in Inorder
        int index = map.get(rootVal);
        
        // 3. Calculate Left Subtree Size (⚡️ 关键点!)
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

[END]

-----

[EN]

## 3\. The Logic: Lowest Common Ancestor (LCA)

  * **Problem**: [LeetCode 236. Lowest Common Ancestor of a Binary Tree](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)
  * **Why**: The pinnacle of **Post-order Traversal** (Bottom-up). You need information from children to make a decision at the parent.

<!-- end list -->

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // 1. Termination: Found p or q, or hit bottom -> return self
        if (root == null || root == p || root == q) return root;

        // 2. Recursion (Go verify left and right subtrees)
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);

        // 3. Logic (Backtracking / Post-order processing)
        
        // If both sides returned non-null, it means p and q are on different sides -> Current is LCA
        if (left != null && right != null) return root; 
        
        // If only one side returned something, pass it up
        if (left != null) return left;   
        if (right != null) return right; 
        
        return null;
    }
}
```

[END]

[ZH]

## 3\. 逻辑：最近公共祖先 (LCA)

  * **对应题目**: [LeetCode 236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)
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

[END]

-----

[EN]

## 4\. The Operation: Delete Node in BST

  * **Problem**: [LeetCode 450. Delete Node in a BST](https://leetcode.cn/problems/delete-node-in-a-bst/)
  * **Why**: The comprehensive exam for BST. It combines **Search** and **Pointer Manipulation**. The tricky part is handling a node with two children.

### Solution 1: Recursive (Standard)

```java
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
            minNode.left = root.left; 
            return root.right; // Replace root with right child
        }
    }
}
```

### Solution 2: Iterative (Hardcore)

```java
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
        if (pre == null) return deleteOneNode(cur);
        
        if (pre.left != null && pre.left.val == key) {
            pre.left = deleteOneNode(cur);
        }
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

[END]

[ZH]

## 4\. 操作：删除 BST 中的节点

  * **对应题目**: [LeetCode 450. 删除二叉搜索树中的节点](https://leetcode.cn/problems/delete-node-in-a-bst/)
  * **理由**: BST 操作的综合大题。结合了 **搜索** 和 **指针操作**。最难的是处理“双子节点”的情况——需要找到右子树的最小值来“继位”。

### 解法 1: 递归法（标准）

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

### 解法 2: 迭代法（硬核，供参考）

*需要手动维护父节点指针。逻辑繁琐，但对理解指针操作非常有帮助。*

```java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null) return null;
        
        TreeNode cur = root;
        TreeNode pre = null; // 父节点指针
        
        // 1. 寻找目标节点
        while (cur != null) {
            if (cur.val == key) break;
            pre = cur;
            if (cur.val > key) cur = cur.left;
            else cur = cur.right;
        }
        if (cur == null) return root; // 没找到

        // 2. 删除逻辑
        // 如果删除的是根节点
        if (pre == null) return deleteOneNode(cur);
        
        // 如果删除的是左孩子
        if (pre.left != null && pre.left.val == key) {
            pre.left = deleteOneNode(cur);
        }
        // 如果删除的是右孩子
        if (pre.right != null && pre.right.val == key) {
            pre.right = deleteOneNode(cur);
        }
        return root;
    }

    // 辅助函数：删除特定节点并重组子树
    private TreeNode deleteOneNode(TreeNode target) {
        if (target == null) return null;
        if (target.right == null) return target.left; // 只有左孩子
        
        // Put target.left to the leftmost of target.right
        // 把左孩子挂到右子树的最左边
        TreeNode cur = target.right;
        while (cur.left != null) cur = cur.left;
        cur.left = target.left;
        
        return target.right;
    }
}
```

[END]
