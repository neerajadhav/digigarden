---
title: 06 - Kruskal's Algorithm
tags:
  - Graphs
  - MST
  - Greedy
  - DSU
---

# Kruskal's Algorithm

Kruskal's Algorithm is a **Greedy** algorithm used to find the **Minimum Spanning Tree (MST)** of a connected, undirected, weighted graph.

## Minimum Spanning Tree (MST)
An MST is a subset of the edges of a connected, edge-weighted undirected graph that connects all the vertices together, without any cycles and with the minimum possible total edge weight.

## Intuition
Kruskal's algorithm builds the MST by picking edges in increasing order of weight. It uses a **Disjoint Set Union (DSU)** data structure to efficiently check if adding an edge would create a cycle.

## Algorithm Steps
1.  **Sort** all the edges in non-decreasing order of their weights.
2.  **Initialize** a DSU structure for $V$ vertices.
3.  Iterate through the sorted edges:
    - If the two vertices of the edge belong to **different sets** (i.e., adding the edge doesn't form a cycle):
      - **Include** the edge in the MST.
      - **Union** the sets of the two vertices.
    - Otherwise, **discard** the edge.

## Java Implementation

### Disjoint Set Union (DSU) Implementation
```java
class DSU {
    int[] parent, rank;
    DSU(int n) {
        parent = new int[n];
        rank = new int[n];
        for (int i = 0; i < n; i++) parent[i] = i;
    }
    int find(int i) {
        if (parent[i] == i) return i;
        return parent[i] = find(parent[i]); // Path Compression
    }
    boolean union(int i, int j) {
        int rootI = find(i);
        int rootJ = find(j);
        if (rootI != rootJ) {
            if (rank[rootI] < rank[rootJ]) parent[rootI] = rootJ;
            else if (rank[rootI] > rank[rootJ]) parent[rootJ] = rootI;
            else {
                parent[rootI] = rootJ;
                rank[rootJ]++;
            }
            return true;
        }
        return false;
    }
}
```

### Kruskal's Algorithm
```java
import java.util.*;

class Edge implements Comparable<Edge> {
    int src, dest, weight;
    Edge(int src, int dest, int weight) {
        this.src = src;
        this.dest = dest;
        this.weight = weight;
    }
    public int compareTo(Edge other) {
        return Integer.compare(this.weight, other.weight);
    }
}

public class Kruskal {
    public static void kruskalMST(int v, List<Edge> edges) {
        Collections.sort(edges);
        DSU dsu = new DSU(v);
        List<Edge> mst = new ArrayList<>();
        int mstWeight = 0;

        for (Edge edge : edges) {
            if (dsu.union(edge.src, edge.dest)) {
                mst.add(edge);
                mstWeight += edge.weight;
            }
        }

        System.out.println("Edges in MST:");
        for (Edge edge : mst) {
            System.out.println(edge.src + " -- " + edge.dest + " == " + edge.weight);
        }
        System.out.println("Total Weight: " + mstWeight);
    }
}
```

## Sample Input and Output

### Input
```text
4 vertices, 5 edges
0 1 10
0 2 6
0 3 5
1 3 15
2 3 4
```

### Output
```text
Edges in MST:
2 -- 3 == 4
0 -- 3 == 5
0 -- 1 == 10
Total Weight: 19
```

## Complexity
| Metric | Complexity | Description |
| :--- | :--- | :--- |
| **Time** | $O(E \log E)$ | Dominating factor is sorting the edges. DSU operations are nearly $O(1)$. |
| **Space** | $O(V + E)$ | To store edges and DSU arrays. |

## Key Takeaways
- **Greedy Strategy**: Always pick the smallest edge that doesn't cause a cycle.
- **Cycle Detection**: DSU is the most efficient way to perform cycle detection in Kruskal's.
- **Disconnected Graphs**: If the graph is not connected, Kruskal's will find a **Minimum Spanning Forest**.
