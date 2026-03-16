---
title: 07 - Prim's Algorithm
tags:
  - Graphs
  - MST
  - Greedy
---

# Prim's Algorithm

Prim's Algorithm is a **Greedy** algorithm used to find the **Minimum Spanning Tree (MST)** of a connected, undirected, weighted graph.

## Intuition
Unlike Kruskal's, which is edge-based, Prim's is **node-based**. It starts from an arbitrary node and grows the MST one vertex at a time by always picking the minimum weight edge that connects a vertex in the MST to a vertex outside the MST.

## Algorithm Steps
1.  **Initialize** a `visited` array and a **Min-Priority Queue** (PQ).
2.  **Pick** an arbitrary starting vertex and add its edges to the PQ.
3.  While the PQ is not empty and we haven't included all vertices:
    - **Extract** the edge with the minimum weight from the PQ.
    - If the destination vertex is already **visited**, skip it.
    - **Include** the edge/vertex in the MST and mark the vertex as visited.
    - **Add** all edges from the newly included vertex to the PQ (if the destination is not visited).

## Java Implementation

### Edge and Node Representation
```java
class Edge {
    int target, weight;
    Edge(int target, int weight) {
        this.target = target;
        this.weight = weight;
    }
}

class Node implements Comparable<Node> {
    int vertex, weight;
    Node(int vertex, int weight) {
        this.vertex = vertex;
        this.weight = weight;
    }
    public int compareTo(Node other) {
        return Integer.compare(this.weight, other.weight);
    }
}
```

### Prim's Algorithm Implementation
```java
import java.util.*;

public class Prim {
    public static void primsMST(int v, List<List<Edge>> adj) {
        boolean[] visited = new boolean[v];
        PriorityQueue<Node> pq = new PriorityQueue<>();
        
        // Start from node 0
        pq.add(new Node(0, 0));
        int totalWeight = 0;
        int edgesCount = 0;

        while (!pq.isEmpty()) {
            Node curr = pq.poll();
            int u = curr.vertex;

            if (visited[u]) continue;

            visited[u] = true;
            totalWeight += curr.weight;
            if (u != 0) edgesCount++; // Don't count the virtual edge to start node

            for (Edge edge : adj.get(u)) {
                if (!visited[edge.target]) {
                    pq.add(new Node(edge.target, edge.weight));
                }
            }
        }

        if (edgesCount == v - 1) {
            System.out.println("Total MST Weight: " + totalWeight);
        } else {
            System.out.println("Graph is not connected!");
        }
    }
}
```

## Sample Input and Output

### Input
```text
5 vertices, 7 edges
0 1 2
0 3 6
1 2 3
1 3 8
1 4 5
2 4 7
3 4 9
```

### Output
```text
Total MST Weight: 16
```
*(MST edges: 0-1 (2), 1-2 (3), 1-4 (5), 0-3 (6))*

## Complexity
| Metric | Complexity | Description |
| :--- | :--- | :--- |
| **Time** | $O(E \log V)$ | Each edge is processed once; PQ operations take $\log V$. |
| **Space** | $O(V + E)$ | To store the adjacency list and priority queue. |

## Kruskal's vs. Prim's
| Feature | Kruskal's | Prim's |
| :--- | :--- | :--- |
| **Approach** | Edge-based | Node-based |
| **Data Structure** | DSU, Sorting | Priority Queue, Adjacency List |
| **Performance** | Better for Sparse graphs | Better for Dense graphs |
| **Handling** | Naturally finds forests | Works on connected components |

## Key Takeaways
- **Greedy Choice**: Always picks the cheapest edge connecting to the current tree.
- **Initial Node**: Can start from any node; the result weight remains the same.
- **Connectivity**: If the graph is disconnected, Prim's only finds the MST of the component containing the starting node.
