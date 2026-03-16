---
title: 44 - Number of Connected Components in an Undirected Graph
tags:
  - Graph
  - Union-Find
  - BFS
  - DFS
  - Medium
---

You have a graph of `n` nodes. You are given an integer `n` and an array `edges` where `edges[i] = [a, b]` indicates that there is an edge between `a` and `b` in the graph.

Return the number of connected components in the graph.

---

## Examples

**Example 1**

![Connected Components Example 1](../../../assets/attachments/count-connected-components-1.avif)

Input: `n = 5, edges = [[0,1],[1,2],[3,4]]`  
Output: `2`

---

**Example 2**

![Connected Components Example 2](../../../assets/attachments/count-connected-components-2.avif)

Input: `n = 5, edges = [[0,1],[1,2],[2,3],[3,4]]`  
Output: `1`

---

## Solution

### Java Code

```java
class Solution {
    public int countComponents(int n, int[][] edges) {
        int[] parent = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }

        int count = n;
        for (int[] edge : edges) {
            int rootX = find(parent, edge[0]);
            int rootY = find(parent, edge[1]);

            if (rootX != rootY) {
                parent[rootX] = rootY;
                count--;
            }
        }

        return count;
    }

    private int find(int[] parent, int i) {
        if (parent[i] == i) {
            return i;
        }
        return parent[i] = find(parent, parent[i]); // Path compression
    }
}
```

---

## Intuition

The problem asks for the number of **disjoint sets** or **connected components** in an undirected graph.

### Union-Find (Disjoint Set Union)
Union-Find is the most efficient and natural choice for this problem.

1.  **Initialize**: Start with `n` components (each node is its own parent).
2.  **Combine**: Iterate through each edge. If the two nodes of an edge are in different components, `union` them and decrement the total component `count`.
3.  **Efficiency**: Using **Path Compression** in the `find` operation and/or **Union by Rank** ensures that operations are nearly $O(1)$.

---

## Complexity Analysis

### Time Complexity

$O(V + E \cdot \alpha(V))$

- $V$ is the number of nodes (`n`), $E$ is the number of edges.
- $\alpha$ is the Inverse Ackermann function, which is effectively constant.

### Space Complexity

$O(V)$

- To store the `parent` array for the `n` nodes.

---

## Key Takeaways

- **Connected components** in an undirected graph are perfectly handled by **Union-Find**.
- This approach is often more memory-efficient than building an adjacency list and running DFS/BFS.
- The same logic applies to many "clustering" or "connectivity" problems.
