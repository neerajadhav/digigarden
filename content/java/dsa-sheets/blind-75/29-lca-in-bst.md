---
title: 29 - Lowest Common Ancestor in BST
tags:
  - Tree
  - Medium
---

## Problem Statement
Given a binary search tree (BST) where all node values are unique, and two nodes from the tree `p` and `q`, return the **lowest common ancestor (LCA)** of the two nodes.

The lowest common ancestor between two nodes `p` and `q` is the lowest node in a tree `T` such that both `p` and `q` are descendants. The ancestor is allowed to be a descendant of itself.

## Examples

### Example 1
![LCA in BST Example 1](../../../assets/attachments/lca-bst-1.avif)

**Input**: `root = [5,3,8,1,4,7,9,null,2], p = 3, q = 8`  
**Output**: `5`  
**Explanation**: Since 3 is in the left subtree and 8 is in the right subtree of node 5, their LCA is 5.

### Example 2
![LCA in BST Example 2](../../../assets/attachments/lca-bst-2.avif)

**Input**: `root = [5,3,8,1,4,7,9,null,2], p = 3, q = 4`  
**Output**: `3`  
**Explanation**: The LCA of nodes 3 and 4 is 3, since a node can be a descendant of itself.

## Solution

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // While the root is not null
        while (root != null) {
            // 1. Both nodes are in the left subtree
            if (p.val < root.val && q.val < root.val) {
                root = root.left;
            } 
            // 2. Both nodes are in the right subtree
            else if (p.val > root.val && q.val > root.val) {
                root = root.right;
            } 
            // 3. We found the split point or one node is the ancestor of the other
            else {
                return root;
            }
        }
        return null;
    }
}
```

## Intuition
The Binary Search Tree (BST) property is the key: for any node, all nodes in the left subtree are smaller, and all nodes in the right subtree are larger.
1. If both `p` and `q` are **smaller** than the current node, their LCA must be in the **left subtree**.
2. If both `p` and `q` are **larger** than the current node, their LCA must be in the **right subtree**.
3. If one is smaller and the other is larger (or if the current node is equal to `p` or `q`), then the current node **is** the Lowest Common Ancestor. This is the "split point".

## Complexity Analysis
- **Time Complexity**: $O(h)$, where $h$ is the height of the tree. In the worst case (skewed tree), $h = n$. In a balanced tree, $h = \log n$.
- **Space Complexity**: $O(1)$ for the iterative approach provided above. For a recursive version, it would be $O(h)$ due to the call stack.

## Key Takeaways
- **BST Property**: Always look for ways to leverage `left < root < right` to avoid exploring subtrees unnecessarily.
- **Top-Down Logic**: Unlike general trees where LCA often involves post-order traversal, BST LCA can be found efficiently with a simple top-down approach.
- **Unique Values**: The assumption of unique values simplifies the comparison logic significantly.
