---
title: 03 - Dijkstra's Algorithm
tags:
  - Graphs
  - Shortest Path
  - Greedy
---

# Dijkstra's Algorithm

Dijkstra's Algorithm finds the shortest path from a single source vertex to all other vertices in a **weighted graph** with non-negative edge weights.

## Intuition
It is a **Greedy** algorithm. At each step, it picks the "closest" unvisited vertex and "relaxes" its neighbors.

## Key Concepts
- **Relaxation**: If we find a path to node `v` through node `u` that is shorter than the current known distance to `v`, we update `dist[v]`.
  - `if (dist[u] + weight(u, v) < dist[v]) dist[v] = dist[u] + weight(u, v)`
- **Priority Queue**: Efficiently retrieves the vertex with the minimum distance.

## Algorithm Steps
1.  **Initialize**: Set `dist[source] = 0` and `dist[v] = ∞` for all other vertices.
2.  **Insert** source into a Min-Priority Queue (PQ).
3.  While PQ is not empty:
    - **Extract** the vertex `u` with the smallest `dist[u]`.
    - For each neighbor `v` of `u`:
      - If `dist[u] + weight(u, v) < dist[v]`:
        - Update `dist[v] = dist[u] + weight(u, v)`.
        - **Push** `(dist[v], v)` into the PQ.

## Java Implementation
Using `PriorityQueue` with a custom `Edge` class for the Adjacency List.

```java
import java.util.*;

class Edge {
    int target, weight;
    Edge(int target, int weight) {
        this.target = target;
        this.weight = weight;
    }
}

class Node implements Comparable<Node> {
    int id, distance;
    Node(int id, int distance) {
        this.id = id;
        this.distance = distance;
    }
    public int compareTo(Node other) {
        return Integer.compare(this.distance, other.distance);
    }
}

public class Dijkstra {
    public static void dijkstra(int source, List<List<Edge>> adj, int n) {
        int[] dist = new int[n];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[source] = 0;

        PriorityQueue<Node> pq = new PriorityQueue<>();
        pq.add(new Node(source, 0));

        while (!pq.isEmpty()) {
            Node curr = pq.poll();
            int u = curr.id;

            if (curr.distance > dist[u]) continue;

            for (Edge edge : adj.get(u)) {
                int v = edge.target;
                if (dist[u] + edge.weight < dist[v]) {
                    dist[v] = dist[u] + edge.weight;
                    pq.add(new Node(v, dist[v]));
                }
            }
        }

        // Output distances
        for (int i = 0; i < n; i++) {
            System.out.println("Node " + i + ": " + (dist[i] == Integer.MAX_VALUE ? "INF" : dist[i]));
        }
    }

    public static void main(String[] args) {
        // ... Graph construction from input
    }
}
```

## Sample Input and Output

### Input
```text
5 6
0 1 2
0 2 4
1 2 1
1 3 7
2 4 3
3 4 1
```
*(Nodes Edges; then u v weight)*

### Output
```text
Node 0: 0
Node 1: 2
Node 2: 3
Node 3: 9
Node 4: 6
```

## Complexity
| Metric | Complexity | Description |
| :--- | :--- | :--- |
| **Time** | $O((V + E) \log V)$ | Each edge relaxation takes $\log V$ in the PQ. |
| **Space** | $O(V + E)$ | To store the graph and distance array. |

## Key Takeaways
- **Greedy Choice**: Picks the local optimal (nearest node) hoping for global optimal shortest paths.
- **Limitation**: Does **NOT** work with negative edge weights. For negative weights, use **Bellman-Ford**.
- **Efficiency**: The Priority Queue is critical for achieving the $O(E \log V)$ performance.
