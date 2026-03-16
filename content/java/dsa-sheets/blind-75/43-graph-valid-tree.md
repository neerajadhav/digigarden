---
title: 43 - Graph Valid Tree
tags:
  - Graph
  - Union-Find
  - BFS
  - DFS
  - Medium
---

Given `n` nodes labeled from `0` to `n - 1` and a list of undirected edges (each edge is a pair of nodes), write a function to check whether these edges make up a valid tree.

---

## Examples

**Example 1**

Input: `n = 5, edges = [[0, 1], [0, 2], [0, 3], [1, 4]]`  
Output: `true`

---

**Example 2**

Input: `n = 5, edges = [[0, 1], [1, 2], [2, 3], [1, 3], [1, 4]]`  
Output: `false`

---

## Solution

### Java Code

```java
class Solution {
    public boolean validTree(int n, int[][] edges) {
        // A tree must have exactly n - 1 edges
        if (edges.length != n - 1) {
            return false;
        }

        // Union-Find to detect cycles and check connectivity
        int[] parent = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }

        for (int[] edge : edges) {
            int rootX = find(parent, edge[0]);
            int rootY = find(parent, edge[1]);

            // If they share the same root, we found a cycle
            if (rootX == rootY) {
                return false;
            }

            // Union
            parent[rootX] = rootY;
        }

        return true;
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

A graph is a **valid tree** if and only if:
1.  It is **fully connected** (no isolated components).
2.  It contains **no cycles**.

For an undirected graph with `n` nodes, these properties imply that the graph must have exactly `n - 1` edges.

### The Union-Find Approach
Since we already know a tree must have `n - 1` edges, we can use Union-Find to check for cycles as we process the edges.

1.  **Initial Check**: If `edges.length != n - 1`, return `false` immediately.
2.  **Cycle Detection**: For each edge, use `find` to check if both nodes already belong to the same component. 
    - If they do, adding this edge would create a **cycle**.
    - If they don't, `union` them into the same component.
3.  **Result**: If we process all `n - 1` edges without finding a cycle, the graph is guaranteed to be connected and acyclic (a valid tree).

---

## Complexity Analysis

### Time Complexity

$O(E \cdot \alpha(V))$

- $E$ is the number of edges, $V$ is the number of vertices.
- $\alpha$ is the Inverse Ackermann function (effectively constant $O(1)$).
- Since $E = V - 1$, this is practically $O(V)$.

### Space Complexity

$O(V)$

- To store the `parent` array for Union-Find.

---

## Key Takeaways

- In an undirected graph, **Tree $\iff$ $n-1$ edges + Connected $\iff$ $n-1$ edges + Acyclic**.
- **Union-Find** is the go-to data structure for connectivity and cycle detection in undirected graphs.
- **Path compression** in Union-Find makes the operations nearly $O(1)$.
