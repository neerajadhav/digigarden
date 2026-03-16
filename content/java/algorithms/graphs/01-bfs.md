---
title: 01 - Breadth-First Search (BFS)
tags:
  - Graphs
  - Traversal
  - BFS
---

# Breadth-First Search (BFS)

Breadth-First Search (BFS) is a graph traversal algorithm that explores all the companion nodes at the present depth prior to moving on to the nodes at the next depth level.

## Intuition
Think of BFS as ripples in a pond. Starting from a source node, the search expands outward level by level. This makes BFS ideal for finding the **shortest path** in an unweighted graph.

## Algorithm Steps
1.  **Initialize** a queue and a `visited` array.
2.  **Enqueue** the starting node and mark it as visited.
3.  While the queue is not empty:
    - **Dequeue** a node.
    - Process the node (e.g., print it).
    - For each **unvisited neighbor**, mark it as visited and **enqueue** it.

## Graph Representation (Adjacency List)
We typically use a `List<List<Integer>>` or `Map<Integer, List<Integer>>` for BFS.

### Java Implementation
```java
import java.util.*;

public class Main {
    public static void bfs(int startNode, List<List<Integer>> adj, int numNodes) {
        boolean[] visited = new boolean[numNodes];
        Queue<Integer> queue = new LinkedList<>();

        visited[startNode] = true;
        queue.add(startNode);

        while (!queue.isEmpty()) {
            int curr = queue.poll();
            System.out.print(curr + " ");

            for (int neighbor : adj.get(curr)) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    queue.add(neighbor);
                }
            }
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int v = sc.nextInt(); // Number of vertices
        int e = sc.nextInt(); // Number of edges

        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < v; i++) adj.add(new ArrayList<>());

        for (int i = 0; i < e; i++) {
            int u = sc.nextInt();
            int w = sc.nextInt();
            adj.get(u).add(w);
            adj.get(w).add(u); // Undirected graph
        }

        System.out.println("BFS Traversal starting from node 0:");
        bfs(0, adj, v);
        sc.close();
    }
}
```

## Sample Input and Output

### Input
```text
5 4
0 1
0 2
1 3
1 4
```

### Output
```text
BFS Traversal starting from node 0:
0 1 2 3 4 
```

## Complexity
| Metric | Complexity | Description |
| :--- | :--- | :--- |
| **Time** | $O(V + E)$ | Every vertex and edge is processed exactly once. |
| **Space** | $O(V)$ | Queue can hold at most $V$ vertices; `visited` array size is $V$. |

## Applications
- Finding the **Shortest Path** in unweighted graphs.
- **Cycle detection** in undirected graphs.
- **Bipartite graph** checking.
- Finding **Connected Components**.

## Key Takeaways
- BFS uses a **Queue** (FIFO).
- Always track **visited** nodes to avoid infinite loops in cyclic graphs.
- It finds the shortest path level by level.
