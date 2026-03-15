---
title: 32 - Kth Smallest Integer in BST
tags:
  - Tree
  - Medium
---

## Problem Statement
Given the `root` of a binary search tree, and an integer `k`, return the **kth smallest value** (1-indexed) in the tree.

A binary search tree satisfies the following constraints:
1. The **left subtree** of every node contains only nodes with keys **less than** the node's key.
2. The **right subtree** of every node contains only nodes with keys **greater than** the node's key.
3. Both the left and right subtrees are also binary search trees.

## Examples

### Example 1
![Kth Smallest Integer in BST Example 1](../../../assets/attachments/kth-smallest-integer-in-bst-1.avif)

**Input**: `root = [2,1,3], k = 1`  
**Output**: `1`  
**Explanation**: The nodes in increasing order are [1, 2, 3]. The 1st smallest is 1.

### Example 2
![Kth Smallest Integer in BST Example 2](../../../assets/attachments/kth-smallest-integer-in-bst-2.avif)

**Input**: `root = [4,3,5,2,null], k = 4`  
**Output**: `5`  
**Explanation**: The nodes in increasing order are [2, 3, 4, 5]. The 4th smallest is 5.

## Solution

```java
import java.util.Stack;

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
    public int kthSmallest(TreeNode root, int k) {
        Stack<TreeNode> stack = new Stack<>();
        TreeNode curr = root;
        
        while (curr != null || !stack.isEmpty()) {
            // 1. Traverse to the leftmost node
            while (curr != null) {
                stack.push(curr);
                curr = curr.left;
            }
            
            // 2. Process node
            curr = stack.pop();
            k--;
            if (k == 0) {
                return curr.val;
            }
            
            // 3. Move to the right child
            curr = curr.right;
        }
        
        return -1; // Should not reach here per constraints
    }
}
```

## Intuition
The defining property of a **Binary Search Tree** is that its **In-order Traversal** (Left -> Root -> Right) visits nodes in strictly increasing order.
1. To find the $k$-th smallest element, we simply need to perform an in-order traversal and return the $k$-th node we encounter.
2. An **iterative approach** using a stack is often preferred as it allows for an "early exit" — as soon as we decrement $k$ to zero, we can return the result immediately without visiting the rest of the tree.

## Complexity Analysis
- **Time Complexity**: $O(h + k)$, where $h$ is the height of the tree. We must descend to the leftmost leaf ($h$) and then process $k$ nodes.
- **Space Complexity**: $O(h)$, used by the stack to store nodes along the path from the root to a leaf. In a balanced tree, $h = \log n$; in a skewed tree, $h = n$.

## Key Takeaways
- **In-order = Sorted**: This is the single most important intuition for BST-related order problems.
- **Iterative DFS**: Using an explicit stack for in-order traversal provides fine-grained control over the iteration process.
- **1-indexed Handling**: Ensure you decrement $k$ *after* reaching the node to correctly handle the 1-based indexing.
