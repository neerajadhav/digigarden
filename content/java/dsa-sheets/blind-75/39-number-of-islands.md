---
title: 39 - Number of Islands
tags:
  - Graph
  - BFS
  - DFS
  - Medium
---

Given a 2D grid `grid` where `'1'` represents land and `'0'` represents water, count and return the number of islands.

An island is formed by connecting adjacent lands horizontally or vertically and is surrounded by water. You may assume water is surrounding the grid (i.e., all the edges are water).

---

## Examples

**Example 1**

Input:

```
grid = [
    ["0","1","1","1","0"],
    ["0","1","0","1","0"],
    ["1","1","0","0","0"],
    ["0","0","0","0","0"]
  ]
```

Output: `1`

---

**Example 2**

Input:

```
grid = [
    ["1","1","0","0","1"],
    ["1","1","0","0","1"],
    ["0","0","1","0","0"],
    ["0","0","0","1","1"]
  ]
```

Output: `4`

---

## Solution

### Java Code

```java
class Solution {
    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0) {
            return 0;
        }

        int count = 0;
        int rows = grid.length;
        int cols = grid[0].length;

        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (grid[i][j] == '1') {
                    count++;
                    dfs(grid, i, j);
                }
            }
        }

        return count;
    }

    private void dfs(char[][] grid, int r, int c) {
        int rows = grid.length;
        int cols = grid[0].length;

        if (r < 0 || c < 0 || r >= rows || c >= cols || grid[r][c] == '0') {
            return;
        }

        grid[r][c] = '0'; // Mark as visited by sinking the island

        dfs(grid, r + 1, c);
        dfs(grid, r - 1, c);
        dfs(grid, r, c + 1);
        dfs(grid, r, c - 1);
    }
}
```

---

## Intuition

The grid can be viewed as an **undirected graph** where each island is a **connected component**. The goal is to count these components.

When we find a piece of land (`'1'`), we know we've found a new island (or a piece of one we haven't visited). We increment our count and then use **DFS (Depth-First Search)** or **BFS (Breadth-First Search)** to "sink" or visit all land cells connected to this one.

### The "Sinking" Technique
Instead of using a separate `visited` array, we can modify the grid in-place by changing `'1'` to `'0'`. This marks the cell as visited and prevents us from counting the same island multiple times.

---

## Complexity Analysis

### Time Complexity

$O(M \times N)$

- $M$ is the number of rows, $N$ is the number of columns.
- We visit each cell at most once.

### Space Complexity

$O(M \times N)$

- In the worst case (e.g., all land), the recursion stack for DFS can go up to $M \times N$ deep.

---

## Key Takeaways

- Grid problems are often **graph problems** in disguise.
- **DFS/BFS** are the primary tools for traversing connected components.
- In-place modification (**sinking**) is a common space-saving trick in grid traversal.
