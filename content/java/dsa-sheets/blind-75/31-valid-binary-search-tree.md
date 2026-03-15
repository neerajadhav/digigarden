---
title: 31 - Valid Binary Search Tree
tags:
  - Tree
  - Medium
---

## Problem Statement
Given the `root` of a binary tree, return `true` if it is a **valid binary search tree (BST)**, otherwise return `false`.

A valid binary search tree satisfies the following constraints:
1. The **left subtree** of every node contains only nodes with keys **less than** the node's key.
2. The **right subtree** of every node contains only nodes with keys **greater than** the node's key.
3. Both the left and right subtrees are also binary search trees.

## Examples

### Example 1
![Valid BST Example 1](../../../assets/attachments/valid-bst-1.avif)

**Input**: `root = [2,1,3]`  
**Output**: `true`  
**Explanation**: All nodes satisfy the BST constraints.

### Example 2
![Valid BST Example 2](../../../assets/attachments/valid-bst-2.avif)

**Input**: `root = [1,2,3]`  
**Output**: `false`  
**Explanation**: The root node's value is 1, but its left child's value is 2, which is greater than 1 (violates the property).

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
    public boolean isValidBST(TreeNode root) {
        return validate(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }
    
    private boolean validate(TreeNode node, long min, long max) {
        // Base case: An empty tree/leaf child is valid
        if (node == null) {
            return true;
        }
        
        // Check current node's value against the valid range
        if (node.val <= min || node.val >= max) {
            return false;
        }
        
        // Recurse:
        // Left child must be in (min, currentVal)
        // Right child must be in (currentVal, max)
        return validate(node.left, min, node.val) && 
               validate(node.right, node.val, max);
    }
}
```

## Intuition
A common pitfall is only checking if a node is greater than its left child and smaller than its right child locally. However, BST dictates that **all** nodes in the left subtree must be smaller than the root.
1. We use a helper function that carries a **valid range** `(min, max)` for each node.
2. For the root, the range is `(Long.MIN_VALUE, Long.MAX_VALUE)`.
3. When moving to the **left**, the new `max` becomes the parent's value.
4. When moving to the **right**, the new `min` becomes the parent's value.
5. If any node's value falls outside this shrinking window, it's not a valid BST.

## Complexity Analysis
- **Time Complexity**: $O(n)$, where $n$ is the number of nodes in the tree. We visit each node exactly once.
- **Space Complexity**: $O(h)$, where $h$ is the height of the tree. This is used by the recursion stack.

## Key Takeaways
- **Global Constraints**: Local checks are insufficient for BST validation; nodes must respect the limits imposed by all ancestors.
- **Handling Limits**: Using `Long` instead of `Integer` for the search boundaries prevents errors when node values are exactly `Integer.MIN_VALUE` or `Integer.MAX_VALUE`.
- **Recursion vs. In-order**: Another way to solve this is to perform an **In-order Traversal** and check if the resulting list is strictly increasing.
