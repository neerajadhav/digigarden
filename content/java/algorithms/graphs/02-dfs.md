---
title: 02 - Depth-First Search (DFS)
tags:
  - Graphs
  - Traversal
  - DFS
---

# Depth-First Search (DFS)

Depth-First Search (DFS) is a graph traversal algorithm that explores as far as possible along each branch before backtracking.

## Intuition
Think of DFS as exploring a maze. You go down one path until you hit a dead end, then backtrack to the last fork in the road and try another path. This makes it naturally **recursive**.

## Algorithm Steps
1.  **Initialize** a `visited` array (or set).
2.  For a starting node `v`:
    - Mark `v` as **visited**.
    - Process `v` (e.g., print it).
    - For each **unvisited neighbor** `u`, recursively call DFS on `u`.

## Graph Representation
DFS works efficiently with both Adjacency Lists and Adjacency Matrices.

### 1. Recursive Implementation
This is the most common way to implement DFS, using the implicit function call stack.

```java
import java.util.*;

public class Main {
    public static void dfsRecursive(int curr, List<List<Integer>> adj, boolean[] visited) {
        visited[curr] = true;
        System.out.print(curr + " ");

        for (int neighbor : adj.get(curr)) {
            if (!visited[neighbor]) {
                dfsRecursive(neighbor, adj, visited);
            }
        }
    }

    public static void main(String[] args) {
        // ... (Graph Initialization same as BFS)
        boolean[] visited = new boolean[numNodes];
        dfsRecursive(0, adj, visited);
    }
}
```

### 2. Iterative Implementation
Uses an explicit **Stack** data structure.

```java
public static void dfsIterative(int startNode, List<List<Integer>> adj, int numNodes) {
    boolean[] visited = new boolean[numNodes];
    Stack<Integer> stack = new Stack<>();

    stack.push(startNode);

    while (!stack.isEmpty()) {
        int curr = stack.pop();

        if (!visited[curr]) {
            visited[curr] = true;
            System.out.print(curr + " ");

            // Add neighbors in reverse for same order as recursion
            List<Integer> neighbors = adj.get(curr);
            for (int i = neighbors.size() - 1; i >= 0; i--) {
                int neighbor = neighbors.get(i);
                if (!visited[neighbor]) {
                    stack.push(neighbor);
                }
            }
        }
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
DFS Traversal starting from node 0:
0 1 3 4 2 
```

## Complexity
| Metric | Complexity | Description |
| :--- | :--- | :--- |
| **Time** | $O(V + E)$ | Every vertex and edge is processed exactly once. |
| **Space** | $O(V)$ | Recursion stack or explicit stack can hold at most $V$ vertices. |

## Applications
- Finding **Connected Components**.
- **Topological Sorting** (using Finish Time).
- **Cycle Detection** in directed and undirected graphs.
- Solving puzzles with only one solution (like certain mazes).
- **Path Finding** between two nodes.

## Key Takeaways
- DFS uses a **Stack** (LIFO), either implicit (recursion) or explicit.
- It explores **depth** first, unlike BFS which explores **breadth**.
- Useful for exhaustive searches and tree-based problems.
