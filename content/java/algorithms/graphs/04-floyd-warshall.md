---
title: 04 - Floyd-Warshall Algorithm
tags:
  - Graphs
  - Shortest Path
  - Dynamic Programming
---

# Floyd-Warshall Algorithm

The Floyd-Warshall algorithm is a dynamic programming algorithm used to find the **shortest paths between all pairs of vertices** in a weighted graph (directed or undirected) with positive or negative edge weights (but no negative cycles).

## Intuition
The core idea is to consider each vertex $k$ as an intermediate point. For every pair of vertices $(i, j)$, we check if going through $k$ results in a shorter path than the current known path from $i$ to $j$.

## Equation
For each vertex $k \in \{1, 2, ..., V\}$:
$dist[i][j] = \min(dist[i][j], dist[i][k] + dist[k][j])$

## Algorithm Steps
1.  **Initialize** a 2D distance matrix `dist[][]` of size $V \times V$.
    - `dist[i][j] = weight(i, j)` if an edge exists.
    - `dist[i][i] = 0`.
    - `dist[i][j] = ∞` otherwise.
2.  **Iterate** through all vertices $k$ from $0$ to $V-1$.
3.  For each $k$, iterate through all pairs $(i, j)$ and update `dist[i][j]` using the equation above.

## Java Implementation
Uses an Adjacency Matrix representation, which is natural for this algorithm.

```java
import java.util.*;

public class FloydWarshall {
    final static int INF = 99999; // Use a value that won't overflow during addition

    public static void floydWarshall(int[][] graph, int V) {
        int[][] dist = new int[V][V];

        // Initialize distance matrix
        for (int i = 0; i < V; i++) {
            for (int j = 0; j < V; j++) {
                dist[i][j] = graph[i][j];
            }
        }

        // Main algorithm
        for (int k = 0; k < V; k++) {
            for (int i = 0; i < V; i++) {
                for (int j = 0; j < V; j++) {
                    if (dist[i][k] + dist[k][j] < dist[i][j]) {
                        dist[i][j] = dist[i][k] + dist[k][j];
                    }
                }
            }
        }

        printSolution(dist, V);
    }

    static void printSolution(int[][] dist, int V) {
        System.out.println("Shortest distances between every pair of vertices:");
        for (int i = 0; i < V; i++) {
            for (int j = 0; j < V; j++) {
                if (dist[i][j] == INF) System.out.print("INF ");
                else System.out.print(dist[i][j] + "   ");
            }
            System.out.println();
        }
    }

    public static void main(String[] args) {
        int[][] graph = {
            {0,   5,  INF, 10},
            {INF, 0,   3, INF},
            {INF, INF, 0,   1},
            {INF, INF, INF, 0}
        };
        floydWarshall(graph, 4);
    }
}
```

## Negative Cycle Detection
If after the algorithm finishes, any diagonal element `dist[i][i]` is negative, then the graph contains a **negative cycle**.

## Sample Input and Output

### Input (Matrix representation)
```text
0 5 INF 10
INF 0 3 INF
INF INF 0 1
INF INF INF 0
```

### Output
```text
0 5 8 9
INF 0 3 4
INF INF 0 1
INF INF INF 0
```

## Complexity
| Metric | Complexity | Description |
| :--- | :--- | :--- |
| **Time** | $O(V^3)$ | Three nested loops iterating over $V$ vertices. |
| **Space** | $O(V^2)$ | To store the 2D distance matrix. |

## Key Takeaways
- **All-Pairs**: Unlike Dijkstra (Source to all), this is All to All.
- **Negative Weights**: It handles negative weights correctly.
- **Dynamic Programming**: It builds the solution by considering increasing sets of intermediate vertices.
- **Limitation**: Avoid using it for very large graphs ($V > 500$) due to the $O(V^3)$ time complexity.
