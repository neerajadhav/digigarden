---
title: 05 - Topological Sort
tags:
  - Graphs
  - DAG
  - Sorting
---

# Topological Sort

Topological Sort is an ordering of vertices in a **Directed Acyclic Graph (DAG)** such that for every directed edge $u \to v$, vertex $u$ comes before $v$ in the ordering.

## Constraints
- **Directed**: The graph must have directed edges.
- **Acyclic**: The graph must not contain any cycles. If a cycle exists, a topological ordering is impossible.

## 1. Kahn's Algorithm (BFS Based)
This approach uses the concept of **In-degree** (number of incoming edges to a vertex).

### Algorithm Steps
1.  **Calculate** the in-degree of all vertices.
2.  **Enqueue** all vertices with in-degree $0$.
3.  While the queue is not empty:
    - **Dequeue** a vertex `u` and add it to the result.
    - For each neighbor `v` of `u`:
      - **Decrement** in-degree of `v`.
      - If in-degree of `v` becomes $0$, **enqueue** `v`.

### Java Implementation
```java
public static List<Integer> kahnTopSort(int v, List<List<Integer>> adj) {
    int[] inDegree = new int[v];
    for (int i = 0; i < v; i++) {
        for (int neighbor : adj.get(i)) inDegree[neighbor]++;
    }

    Queue<Integer> q = new LinkedList<>();
    for (int i = 0; i < v; i++) {
        if (inDegree[i] == 0) q.add(i);
    }

    List<Integer> result = new ArrayList<>();
    while (!q.isEmpty()) {
        int curr = q.poll();
        result.add(curr);

        for (int neighbor : adj.get(curr)) {
            inDegree[neighbor]--;
            if (inDegree[neighbor] == 0) q.add(neighbor);
        }
    }
    
    // If result size < v, there was a cycle
    return result;
}
```

## 2. DFS-Based Approach
This approach uses the standard DFS traversal and a **Stack** to store nodes after their neighbors have been fully processed.

### Algorithm Steps
1.  Perform DFS starting from an unvisited node.
2.  Before returning from the recursive call (after processing all neighbors), **Push** the current node onto a stack.
3.  After visiting all nodes, pop elements from the stack to get the topological order.

### Java Implementation
```java
public static void dfsSort(int curr, List<List<Integer>> adj, boolean[] visited, Stack<Integer> stack) {
    visited[curr] = true;

    for (int neighbor : adj.get(curr)) {
        if (!visited[neighbor]) {
            dfsSort(neighbor, adj, visited, stack);
        }
    }
    stack.push(curr);
}

public static List<Integer> getTopSortDFS(int v, List<List<Integer>> adj) {
    Stack<Integer> stack = new Stack<>();
    boolean[] visited = new boolean[v];

    for (int i = 0; i < v; i++) {
        if (!visited[i]) dfsSort(i, adj, visited, stack);
    }

    List<Integer> result = new ArrayList<>();
    while (!stack.isEmpty()) result.add(stack.pop());
    return result;
}
```

## Sample Input and Output

### Input (DAG)
```text
6 6
5 2
5 0
4 0
4 1
2 3
3 1
```

### Output
```text
Topological Order: 5 4 2 3 0 1
```

## Complexity
| Metric | Complexity | Description |
| :--- | :--- | :--- |
| **Time** | $O(V + E)$ | Every node and edge is visited exactly once. |
| **Space** | $O(V)$ | To store the in-degrees, queue/stack, and visited array. |

## Applications
- **Task Scheduling**: Determining the order of tasks with dependencies.
- **Build Systems**: Resolving dependencies (e.g., Maven, NPM).
- **Instruction Scheduling** in compilers.
- **Data Serialization** ordering.

## Key Takeaways
- Topological sort is **not unique**.
- **Cycle Detection**: Kahn's algorithm naturally detects cycles if the result size is less than $V$.
- **DAG is Required**: Always ensure the graph has no cycles before expecting a valid sort.
