---
title: 27 - Same Binary Tree
tags:
  - Tree
  - Easy
---

## Problem Statement
Given the roots of two binary trees `p` and `q`, return `true` if the trees are equivalent, otherwise return `false`.

Two binary trees are considered equivalent if they share the exact same structure and the nodes have the same values.

## Examples

### Example 1
![Same Binary Tree Example 1](../../../assets/attachments/same-binary-tree-1.avif)

**Input**: `p = [1,2,3], q = [1,2,3]`  
**Output**: `true`  
**Explanation**: Both trees have the same structure and values.

### Example 2
![Same Binary Tree Example 2](../../../assets/attachments/same-binary-tree-2.avif)

**Input**: `p = [4,7], q = [4,null,7]`  
**Output**: `false`  
**Explanation**: The structures are different; `q` has a `null` left child for the root.

### Example 3
![Same Binary Tree Example 3](../../../assets/attachments/same-binary-tree-3.avif)

**Input**: `p = [1,2,3], q = [1,3,2]`  
**Output**: `false`  
**Explanation**: The structures are the same, but the values in the children are swapped.

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
    public boolean isSameTree(TreeNode p, TreeNode q) {
        // 1. Both are null: identical
        if (p == null && q == null) {
            return true;
        }
        
        // 2. One is null, or values differ: not identical
        if (p == null || q == null || p.val != q.val) {
            return false;
        }
        
        // 3. Recurse for left and right subtrees
        return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
    }
}
```

## Intuition
To determine if two trees are identical, we must verify three conditions at every corresponding node pair:
1. The **values** of the current nodes must be equal.
2. The **left subtrees** must be identical.
3. The **right subtrees** must be identical.

The recursion naturally handles this by breaking the problem down: a tree is the same if its root is the same and its respective children are the same. Base cases are crucial: two `null` nodes are considered the same, while a mismatch between a `null` and a non-null node (or value inequality) results in `false`.

## Complexity Analysis
- **Time Complexity**: $O(min(n, m))$, where $n$ and $m$ are the number of nodes in trees `p` and `q`. We traverse both trees until we find a mismatch or reach the end of the smaller tree.
- **Space Complexity**: $O(h)$, where $h$ is the minimum height of the two trees. This space is used by the recursion stack.

## Key Takeaways
- **Structural Identity**: This problem emphasizes that structural equality is as important as value equality in tree comparisons.
- **Logical ANDing**: The result is only `true` if all sub-comparisons (value, left, right) evaluate to `true`.
- **Early Exit**: The algorithm stops as soon as a mismatch is found, which is an efficient use of recursion.
