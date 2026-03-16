---
title: 40 - Clone Graph
tags:
  - Graph
  - BFS
  - DFS
  - Hash Table
  - Medium
---

Given a node in a **connected undirected graph**, return a **deep copy** of the graph.

Each node in the graph contains an integer value (`val`) and a list of its neighbors.

```java
class Node {
    public int val;
    public List<Node> neighbors;
}
```

---

## Examples

**Example 1**

![Clone Graph Example 1](../../../assets/attachments/clone-graph-1.avif)

Input: `adjList = [[2,4],[1,3],[2,4],[1,3]]`  
Output: `[[2,4],[1,3],[2,4],[1,3]]`  

---

**Example 2**

![Clone Graph Example 2](../../../assets/attachments/clone-graph-2.avif)

Input: `adjList = [[]]`  
Output: `[[]]`  

---

## Solution

### Java Code

```java
import java.util.*;

class Solution {
    public Node cloneGraph(Node node) {
        if (node == null) return null;

        // Map to keep track of cloned nodes: Original Node -> Cloned Node
        Map<Node, Node> map = new HashMap<>();
        
        return clone(node, map);
    }

    private Node clone(Node node, Map<Node, Node> map) {
        // If node already cloned, return the clone
        if (map.containsKey(node)) {
            return map.get(node);
        }

        // Create a new node (clone)
        Node newNode = new Node(node.val);
        map.put(node, newNode);

        // Recursively clone neighbors
        for (Node neighbor : node.neighbors) {
            newNode.neighbors.add(clone(neighbor, map));
        }

        return newNode;
    }
}
```

---

## Intuition

Cloning a graph is a traversal problem. We need to visit every node and every edge exactly once to create a deep copy.

The tricky part is **cycles** and **multiple edges** to the same node. If we simply recurse, we'll end up in an infinite loop or create multiple copies of the same node.

### The Strategy: Mapping
We use a **HashMap** to store the mapping from the **original node** to its **cloned version**.

1.  When we visit a node:
    - If it's already in our map, we've already cloned it (or are in the process of cloning its neighbors). Just return the existing clone.
    - If it's NOT in our map, create a new node with the same value and put it in the map.
2.  Then, iterate through the original node's neighbors and recursively clone them, adding the results to the new node's neighbor list.

---

## Complexity Analysis

### Time Complexity

$O(V + E)$

- $V$ is the number of vertices (nodes) and $E$ is the number of edges.
- Each node and edge is visited exactly once.

### Space Complexity

$O(V)$

- The HashMap stores $V$ nodes.
- The recursion stack can go up to $V$ deep in the worst case (e.g., a path graph).

---

## Key Takeaways

- Use a **HashMap** as a "lookup table" for object identity during deep cloning.
- This pattern (DFS/BFS + Map) is common for any **deep copy** of a structure with arbitrary pointers/references (like linked lists with random pointers).
- Graphs require careful handling of **cycles** to avoid infinite loops.
