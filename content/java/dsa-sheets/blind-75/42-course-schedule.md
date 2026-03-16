---
title: 42 - Course Schedule
tags:
  - Graph
  - BFS
  - Topological Sort
  - Medium
---

You are given an array `prerequisites` where `prerequisites[i] = [a, b]` indicates that you must take course `b` first if you want to take course `a`.

- The pair `[0, 1]` indicates that you must take course `1` before taking course `0`.

There are a total of `numCourses` courses you are required to take, labeled from `0` to `numCourses - 1`.

Return `true` if it is possible to finish all courses, otherwise return `false`.

---

## Examples

**Example 1**

Input: `numCourses = 2, prerequisites = [[0,1]]`  
Output: `true`  
Explanation: First take course 1 (no prerequisites) and then take course 0.

---

**Example 2**

Input: `numCourses = 2, prerequisites = [[0,1],[1,0]]`  
Output: `false`  
Explanation: In order to take course 1 you must take course 0, and to take course 0 you must take course 1. So it is impossible.

---

## Solution

### Java Code

```java
import java.util.*;

class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        // Build Adjacency List and In-Degree Array
        List<List<Integer>> adj = new ArrayList<>();
        int[] inDegree = new int[numCourses];
        
        for (int i = 0; i < numCourses; i++) {
            adj.add(new ArrayList<>());
        }
        
        for (int[] pre : prerequisites) {
            int course = pre[0];
            int prerequisite = pre[1];
            adj.get(prerequisite).add(course);
            inDegree[course]++;
        }
        
        // Kahn's Algorithm (BFS for Topological Sort)
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            if (inDegree[i] == 0) {
                queue.offer(i);
            }
        }
        
        int count = 0;
        while (!queue.isEmpty()) {
            int current = queue.poll();
            count++;
            
            for (int neighbor : adj.get(current)) {
                inDegree[neighbor]--;
                if (inDegree[neighbor] == 0) {
                    queue.offer(neighbor);
                }
            }
        }
        
        // If we processed all nodes, there is no cycle
        return count == numCourses;
    }
}
```

---

## Intuition

This problem is about finding if a **Directed Acyclic Graph (DAG)** can be formed. If there is a cycle in the prerequisites, it is impossible to complete all courses.

### Kahn's Algorithm (BFS)
Kahn's algorithm is a systematic way to find a **Topological Sort** of a directed graph.

1.  **In-Degree**: Calculate the "in-degree" (number of incoming edges) for every node.
2.  **Queue**: Start with all nodes that have an in-degree of `0` (courses with no prerequisites).
3.  **Traversal**: 
    - Pull a node from the queue and "remove" it from the graph by decrementing the in-degree of its neighbors.
    - If a neighbor's in-degree becomes `0`, add it to the queue.
4.  **Result**: If the total number of processed nodes equals `numCourses`, we've successfully topologically sorted the graph (meaning no cycles exist).

---

## Complexity Analysis

### Time Complexity

$O(V + E)$

- $V$ is the number of courses (`numCourses`), and $E$ is the number of prerequisites.
- We build the adjacency list and in-degree array in $O(E)$, and the BFS visits every node and edge once.

### Space Complexity

$O(V + E)$

- $O(V + E)$ to store the adjacency list.
- $O(V)$ for the in-degree array and the BFS queue.

---

## Key Takeaways

- Prerequisites and dependencies are the classic use case for **Directed Graphs**.
- **Cycle detection** in a directed graph is equivalent to checking if a **Topological Sort** is possible.
- **Kahn's Algorithm** is generally preferred for its iterative nature and clear conceptual mapping to "processing" nodes with no dependencies.
