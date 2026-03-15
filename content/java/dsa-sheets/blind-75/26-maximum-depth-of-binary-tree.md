---
title: 26 - Maximum Depth of Binary Tree
tags:
  - Tree
  - Easy
---

## Problem Statement
Given the `root` of a binary tree, return its **maximum depth**.

The depth of a binary tree is defined as the number of nodes along the longest path from the root node down to the farthest leaf node.

## Examples

### Example 1
![Maximum Depth of Binary Tree](../../../assets/attachments/max-depth-of-binary-tree.avif)

**Input**: `root = [1,2,3,null,null,4]`  
**Output**: `3`  
**Explanation**: The longest path is 1 -> 3 -> 4, which contains 3 nodes.

### Example 2
**Input**: `root = []`  
**Output**: `0`  
**Explanation**: An empty tree has a depth of 0.

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
    public int maxDepth(TreeNode root) {
        // Base case: if the node is null, the depth is 0
        if (root == null) {
            return 0;
        }

        // Recursive step: 1 (current node) + max depth of subtrees
        int leftDepth = maxDepth(root.left);
        int rightDepth = maxDepth(root.right);

        return 1 + Math.max(leftDepth, rightDepth);
    }
}
```

## Intuition
The maximum depth of a tree is determined by the height of its subtrees. 
1. If a node is `null`, it contributes `0` to the depth.
2. Otherwise, the depth from any node is $1$ (for the node itself) plus the **maximum** of the depths of its left and right subtrees.
3. This is a classic application of **Depth First Search (DFS)** in a recursive manner.

## Complexity Analysis
- **Time Complexity**: $O(n)$, where $n$ is the total number of nodes in the tree. We visit each node once.
- **Space Complexity**: $O(h)$, where $h$ is the height of the tree. This is the space taken by the recursion stack. In a balanced tree, $h = \log n$; in a skewed tree, $h = n$.

## Key Takeaways
- **DFS Strategy**: Recursion naturally explores the full depth of one branch before backtracking, making it perfect for depth calculations.
- **Base Case**: Always handle the `null` root case to prevent infinite recursion or null pointer exceptions.
- **Optimization**: While recursion is straightforward, this can also be solved iteratively using a Queue (BFS/Level Order Traversal), where the depth would be the number of levels.
