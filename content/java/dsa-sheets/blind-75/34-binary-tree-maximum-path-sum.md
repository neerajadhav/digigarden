---
title: 34 - Binary Tree Maximum Path Sum
tags:
  - Tree
  - Hard
---

## Problem Statement
Given the root of a non-empty binary tree, return the **maximum path sum** of any non-empty path.

A path is a sequence of nodes where each adjacent pair has an edge, and no node appears more than once. The path sum is the sum of node values. A path can stay within a subtree and does not need to pass through the root.

## Examples

### Example 1
![Maximum Path Sum Example 1](../../../assets/attachments/binary-tree-maximum-path-sum-1.avif)

**Input**: `root = [1,2,3]`  
**Output**: `6`  
**Explanation**: The path is 2 -> 1 -> 3 with sum 2 + 1 + 3 = 6.

### Example 2
![Maximum Path Sum Example 2](../../../assets/attachments/binary-tree-maximum-path-sum-2.avif)

**Input**: `root = [-15,10,20,null,null,15,5,-5]`  
**Output**: `40`  
**Explanation**: The optimal path is 15 -> 20 -> 5 with sum 15 + 20 + 5 = 40.

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
    private int maxSum;

    public int maxPathSum(TreeNode root) {
        maxSum = Integer.MIN_VALUE;
        gainFromSubtree(root);
        return maxSum;
    }

    private int gainFromSubtree(TreeNode node) {
        if (node == null) {
            return 0;
        }

        // 1. Calculate max gain from subtrees. Use 0 if the path sum is negative.
        int leftGain = Math.max(gainFromSubtree(node.left), 0);
        int rightGain = Math.max(gainFromSubtree(node.right), 0);

        // 2. The max path through the current node as the "turning point"
        int currentPathSum = node.val + leftGain + rightGain;

        // 3. Update the global maximum path sum
        maxSum = Math.max(maxSum, currentPathSum);

        // 4. Return the maximum gain this node can contribute to its parent
        return node.val + Math.max(leftGain, rightGain);
    }
}
```

## Intuition
This problem is challenging because a path doesn't have to start or end at the root or even pass through it. 
1. We use a **Bottom-Up (Post-order)** approach. For each node, we want to know two things:
   - What is the maximum sum path that "passes through" this node and goes to its parent? (Either `node + leftGain` or `node + rightGain`).
   - What is the maximum sum path where this node is the **highest point** (the "turning point")? (`node + leftGain + rightGain`).
2. We maintain a global variable `maxSum` to track the second case across the entire tree.
3. Crucially, if a subtree's maximum gain is negative, we treat it as `0` (i.e., we don't include it in our path).

## Complexity Analysis
- **Time Complexity**: $O(n)$, where $n$ is the number of nodes. We visit each node exactly once.
- **Space Complexity**: $O(h)$, where $h$ is the height of the tree, used by the recursive stack.

## Key Takeaways
- **Global Tracking**: Sometimes a recursive function needs to return one value to its caller while updating another "global" result (the `maxSum`).
- **Post-order DFS**: Solving for children first (gain) before processing the current node is a standard tree pattern.
- **Pruning Negatives**: `Math.max(gain, 0)` is a powerful way to ignore subtrees that would only decrease our total sum.
