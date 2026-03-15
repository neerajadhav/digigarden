---
title: 30 - Binary Tree Level Order Traversal
tags:
  - Tree
  - Medium
---

## Problem Statement
Given a binary tree `root`, return the **level order traversal** of it as a nested list. Each sublist should contain the values of nodes at a particular level, from left to right.

## Examples

### Example 1
![Binary Tree Level Order Traversal](../../../assets/attachments/binary-tree-level-order-traversal.avif)

**Input**: `root = [1,2,3,4,5,6,7]`  
**Output**: `[[1],[2,3],[4,5,6,7]]`  
**Explanation**: Level 0: [1], Level 1: [2,3], Level 2: [4,5,6,7].

### Example 2
**Input**: `root = [1]`  
**Output**: `[[1]]`

### Example 3
**Input**: `root = []`  
**Output**: `[]`

## Solution

```java
import java.util.*;

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
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) {
            return result;
        }

        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);

        while (!queue.isEmpty()) {
            int levelSize = queue.size();
            List<Integer> currentLevel = new ArrayList<>();

            for (int i = 0; i < levelSize; i++) {
                TreeNode currentNode = queue.poll();
                currentLevel.add(currentNode.val);

                if (currentNode.left != null) {
                    queue.add(currentNode.left);
                }
                if (currentNode.right != null) {
                    queue.add(currentNode.right);
                }
            }
            result.add(currentLevel);
        }

        return result;
    }
}
```

## Intuition
Level order traversal is synonymous with **Breadth First Search (BFS)** on a tree. 
1. We use a **Queue** to keep track of nodes to visit.
2. To group nodes by level, we measure the **queue size** at the start of each level's processing. This size represents exactly how many nodes are at that specific depth.
3. We then iterate exactly `levelSize` times, popping nodes from the queue, adding their values to a temporary list, and pushing their non-null children onto the queue for the *next* level.
4. Once a level is fully processed, we add the list to our final result.

## JCF Components Used
- `java.util.List` & `java.util.ArrayList`: For storing the nested list of results.
- `java.util.Queue` & `java.util.LinkedList`: For the FIFO processing of nodes level by level.

## Complexity Analysis
- **Time Complexity**: $O(n)$, where $n$ is the total number of nodes in the tree. We visit each node exactly once.
- **Space Complexity**: $O(w)$, where $w$ is the maximum width of the tree. In a perfect binary tree, the maximum width is $n/2$ (the leaf level), which is $O(n)$.

## Key Takeaways
- **BFS Pattern**: This is the template for BFS on trees and graphs. Using the queue size is a common trick to process nodes in "waves" or "layers".
- **Empty Case**: Always handle `root == null` separately to avoid runtime errors.
- **Queue Implementation**: `new LinkedList<>()` is a standard way to instantiate a `Queue` in Java.
