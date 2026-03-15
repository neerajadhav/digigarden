---
title: 33 - Construct Binary Tree from Preorder and Inorder Traversal
tags:
  - Tree
  - Medium
---

## Problem Statement
Given two integer arrays `preorder` and `inorder`, rebuild the binary tree and return its root.

- `preorder` is the preorder traversal (Root -> Left -> Right).
- `inorder` is the inorder traversal (Left -> Root -> Right).
- Both arrays consist of the same unique values.

## Examples

### Example 1
![Binary Tree Reconstruction Example 1](../../../assets/attachments/binary-tree-from-preorder-and-inorder-traversal.avif)

**Input**: `preorder = [1,2,3,4], inorder = [2,1,3,4]`  
**Output**: `[1,2,3,null,null,null,4]`  
**Explanation**: The root is 1 (first in preorder). In inorder, 2 is to the left of 1, and [3, 4] are to the right.

### Example 2
**Input**: `preorder = [1], inorder = [1]`  
**Output**: `[1]`

## Solution

```java
import java.util.HashMap;
import java.util.Map;

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
    private int preorderIndex;
    private Map<Integer, Integer> inorderIndexMap;

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        preorderIndex = 0;
        // Build a hashmap to store value -> index relations for inorder array
        inorderIndexMap = new HashMap<>();
        for (int i = 0; i < inorder.length; i++) {
            inorderIndexMap.put(inorder[i], i);
        }

        return arrayToTree(preorder, 0, inorder.length - 1);
    }

    private TreeNode arrayToTree(int[] preorder, int left, int right) {
        // If there are no elements to construct the tree
        if (left > right) {
            return null;
        }

        // Select the preorderIndex element as the root and increment it
        int rootValue = preorder[preorderIndex++];
        TreeNode root = new TreeNode(rootValue);

        // Build left and right subtree
        // excluding inorderIndexMap[rootValue] element because it's the root
        root.left = arrayToTree(preorder, left, inorderIndexMap.get(rootValue) - 1);
        root.right = arrayToTree(preorder, inorderIndexMap.get(rootValue) + 1, right);

        return root;
    }
}
```

## Intuition
Reconstructing a tree requires two traversals because one traversal is often ambiguous. 
1. **Preorder** always gives us the **root** as the first element.
2. Once we have the root, we look into the **Inorder** array. Everything to the **left** of the root in the inorder array belongs to the **left subtree**, and everything to the **right** belongs to the **right subtree**.
3. By recursively applying this, we can partition the arrays and rebuild the structure.
4. Using a **HashMap** to store the indices of the inorder array allows us to find the split point in $O(1)$ time, optimizing the overall process.

## JCF Components Used
- `java.util.Map` & `java.util.HashMap`: To store the value-to-index mapping for the `inorder` array for $O(1)$ lookups.

## Complexity Analysis
- **Time Complexity**: $O(n)$, where $n$ is the number of nodes. We visit each node once, and the index lookup in the `inorder` array is $O(1)$ due to the hash map.
- **Space Complexity**: $O(n)$. This includes $O(n)$ for the hash map and $O(h)$ (where $h$ is the tree height) for the recursive call stack.

## Key Takeaways
- **Global Index**: A global (or member) `preorderIndex` helps keep track of the current root as we move through the preorder array.
- **Partitioning**: The fundamental logic is partitioning the inorder array into left/right segments based on the current root.
- **Uniqueness**: This algorithm relies on the fact that node values are unique; otherwise, wait, the hashmap would fail because keys must be unique!
