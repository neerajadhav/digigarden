---
title: 35 - Serialize and Deserialize Binary Tree
tags:
  - Tree
  - Hard
---

## Problem Statement
Implement an algorithm to **serialize** and **deserialize** a binary tree. 

- **Serialization** is the process of converting a tree into a string representation.
- **Deserialization** is the process of reconstructing the tree from that string.

There are no restrictions on the format, as long as a tree can be flattened and rebuilt accurately.

## Examples

### Example 1
![Serialize and Deserialize Binary Tree](../../../assets/attachments/serialize-and-deserialize-binary-tree.avif)

**Input**: `root = [1,2,3,null,null,4,5]`  
**Output**: `[1,2,3,null,null,4,5]`

### Example 2
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
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Codec {

    // Encodes a tree to a single string using BFS.
    public String serialize(TreeNode root) {
        if (root == null) return "N";
        
        StringBuilder sb = new StringBuilder();
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        
        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();
            if (node == null) {
                sb.append("N,");
                continue;
            }
            sb.append(node.val).append(",");
            queue.add(node.left);
            queue.add(node.right);
        }
        
        return sb.toString();
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if (data.equals("N")) return null;
        
        String[] vals = data.split(",");
        TreeNode root = new TreeNode(Integer.parseInt(vals[0]));
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        
        int i = 1;
        while (!queue.isEmpty() && i < vals.length) {
            TreeNode node = queue.poll();
            
            // Handle Left Child
            if (!vals[i].equals("N")) {
                node.left = new TreeNode(Integer.parseInt(vals[i]));
                queue.add(node.left);
            }
            i++;
            
            // Handle Right Child
            if (i < vals.length && !vals[i].equals("N")) {
                node.right = new TreeNode(Integer.parseInt(vals[i]));
                queue.add(node.right);
            }
            i++;
        }
        
        return root;
    }
}
```

## Intuition
To systematically flatten and rebuild a tree, **Breadth First Search (BFS)** is highly effective.
1. **Serialization**: We traverse the tree level by level. If a node is `null`, we append a placeholder (like "N"). This ensures we capture the exact structure, including empty children.
2. **Deserialization**: We split the string back into an array of values. Using a queue, we reconstruct the tree level by level. For each parent in the queue, the next two values in our array represent its left and right children.

## JCF Components Used
- `java.util.Queue` & `java.util.LinkedList`: For the BFS traversal and reconstruction.
- `java.lang.StringBuilder`: Efficiently builds the serialized string.

## Complexity Analysis
- **Time Complexity**: $O(n)$, where $n$ is the number of nodes. We visit each node once during both processes.
- **Space Complexity**: $O(n)$ for the queue and the storage of serialized/deserialized data.

## Key Takeaways
- **Placeholder Strategy**: Using a special character for `null` is the industry standard for string-based tree representations.
- **Symmetric Logic**: BFS for serialization naturally leads to BFS for deserialization.
- **Memory Efficiency**: `StringBuilder` is strictly necessary here to avoid $O(n^2)$ string concatenation in Java.
