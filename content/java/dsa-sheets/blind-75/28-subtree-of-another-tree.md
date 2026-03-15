---
title: 28 - Subtree of Another Tree
tags:
  - Tree
  - Easy
---

## Problem Statement
Given the roots of two binary trees `root` and `subRoot`, return `true` if there is a subtree of `root` with the same structure and node values of `subRoot` and `false` otherwise.

A subtree of a binary tree `tree` is a tree that consists of a node in `tree` and all of this node's descendants. The tree `tree` could also be considered as a subtree of itself.

## Examples

### Example 1
![Subtree of Another Tree Example 1](../../../assets/attachments/subtree-of-another-1.avif)

**Input**: `root = [1,2,3,4,5], subRoot = [2,4,5]`  
**Output**: `true`  
**Explanation**: The node with value 2 in the main tree is the root of a subtree that matches `subRoot`.

### Example 2
![Subtree of Another Tree Example 2](../../../assets/attachments/subtree-of-another-2.avif)

**Input**: `root = [1,2,3,4,5,null,null,6], subRoot = [2,4,5]`  
**Output**: `false`  
**Explanation**: Although the structure looks similar, the node with value 5 has a descendant (6), so the subtree at 2 is not identical to `subRoot`.

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
    public boolean isSubtree(TreeNode root, TreeNode subRoot) {
        // 1. If main tree is null, no subtree can exist
        if (root == null) {
            return false;
        }
        
        // 2. Check if trees are identical starting from current root
        if (isSameTree(root, subRoot)) {
            return true;
        }
        
        // 3. Recurse down the main tree
        return isSubtree(root.left, subRoot) || isSubtree(root.right, subRoot);
    }
    
    private boolean isSameTree(TreeNode p, TreeNode q) {
        if (p == null && q == null) return true;
        if (p == null || q == null || p.val != q.val) return false;
        
        return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
    }
}
```

## Intuition
A binary tree `subRoot` is a subtree of `root` if it is identical to a tree starting at *some* node within `root`.
1. We define a helper function `isSameTree` to check if two trees are exactly the same (structure and values).
2. Starting from the root of the main tree, we check if `isSameTree(root, subRoot)` is true.
3. If not, we recursively check if `subRoot` is a subtree of `root.left` or `root.right`.
4. This **Double Recursion** approach works by visiting every node in the main tree and performing an equality check.

## Complexity Analysis
- **Time Complexity**: $O(n \times m)$, where $n$ is the number of nodes in `root` and $m$ is the number of nodes in `subRoot`. In the worst case, for every node in `root`, we might check up to $m$ nodes for equality.
- **Space Complexity**: $O(h)$, where $h$ is the height of the main tree. This space is used by the recursion stack.

## Key Takeaways
- **Reusability**: This problem is a perfect example of reusing a simpler algorithm (`isSameTree`) as a building block for a more complex one.
- **Base Case nuances**: We must return `false` if `root` is null (unless `subRoot` is also null, but constraints usually ensure `subRoot` exists). 
- **Logical OR**: Using `||` in the return statement allows us to short-circuit as soon as a subtree match is found anywhere in the main tree.
