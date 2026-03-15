---
title: 25 - Invert Binary Tree
tags:
  - Tree
  - Easy
---

## Problem Statement
Given the `root` of a binary tree, invert the tree and return its root. Inverting a tree means swapping everyone's left and right children.

## Examples

### Example 1
![Invert Binary Tree Example 1](../../../assets/attachments/invert-binary-tree-1.avif)

**Input**: `root = [1,2,3,4,5,6,7]`  
**Output**: `[1,3,2,7,6,5,4]`  
**Explanation**: Every node's left and right children are swapped. For instance, the root 1 had 2 on the left and 3 on the right; after inversion, it has 3 on the left and 2 on the right.

### Example 2
![Invert Binary Tree Example 2](../../../assets/attachments/invert-binary-tree-2.avif)

**Input**: `root = [3,2,1]`  
**Output**: `[3,1,2]`

### Example 3
**Input**: `root = []`  
**Output**: `[]`

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
    public TreeNode invertTree(TreeNode root) {
        // Base case: if the tree is empty
        if (root == null) {
            return null;
        }

        // Recursive step: swap children
        TreeNode temp = root.left;
        root.left = invertTree(root.right);
        root.right = invertTree(temp);

        return root;
    }
}
```

## Intuition
The problem is naturally recursive. To invert a tree, we need to:
1. Invert the **left subtree**.
2. Invert the **right subtree**.
3. Swap the root's **left child** with its **right child**.

The base case for this recursion is when we reach a `null` node (a leaf's child), at which point we simply return `null`.

## Complexity Analysis
- **Time Complexity**: $O(n)$, where $n$ is the number of nodes in the tree. We must visit every node exactly once to perform the swap.
- **Space Complexity**: $O(h)$, where $h$ is the height of the tree. This space is used by the recursion stack. In the worst case (a skewed tree), $h = n$. In the best case (a balanced tree), $h = \log n$.

## Key Takeaways
- **Recursive Thinking**: Trees are recursive data structures, so many tree problems can be solved elegantly using recursion.
- **Bottom-Up vs Top-Down**: The provided solution works top-down (swapping as it descends), but the result is the same regardless of the traversal order as long as every node is processed.
- **In-Place Modification**: This solution modifies the original tree structure rather than creating a new one, which is memory efficient.
