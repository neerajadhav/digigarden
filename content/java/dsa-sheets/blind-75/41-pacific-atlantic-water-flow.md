---
title: 41 - Pacific Atlantic Water Flow
tags:
  - Graph
  - BFS
  - DFS
  - Medium
---

You are given a rectangular island `heights` where `heights[r][c]` represents the height above sea level of the cell at coordinate $(r, c)$.

The island borders the **Pacific Ocean** from the top and left sides, and borders the **Atlantic Ocean** from the bottom and right sides.

Water can flow in four directions (up, down, left, or right) from a cell to a neighboring cell with height equal or lower. Water can also flow into the ocean from cells adjacent to the ocean.

Find all cells where water can flow from that cell to **both** the Pacific and Atlantic oceans.

---

## Examples

**Example 1**

![Pacific Atlantic Water Flow](../../../assets/attachments/pacific-atlantic-water-flow.avif)

Input:

```
heights = [
  [4,2,7,3,4],
  [7,4,6,4,7],
  [6,3,5,3,6]
]
```

Output: `[[0,2],[0,4],[1,0],[1,1],[1,2],[1,3],[1,4],[2,0]]`

---

## Solution

### Java Code

```java
import java.util.*;

class Solution {
    public List<List<Integer>> pacificAtlantic(int[][] heights) {
        int rows = heights.length;
        int cols = heights[0].length;
        
        boolean[][] pacific = new boolean[rows][cols];
        boolean[][] atlantic = new boolean[rows][cols];
        
        // Rows: Top (Pacific) and Bottom (Atlantic)
        for (int c = 0; c < cols; c++) {
            dfs(heights, 0, c, Integer.MIN_VALUE, pacific);
            dfs(heights, rows - 1, c, Integer.MIN_VALUE, atlantic);
        }
        
        // Columns: Left (Pacific) and Right (Atlantic)
        for (int r = 0; r < rows; r++) {
            dfs(heights, r, 0, Integer.MIN_VALUE, pacific);
            dfs(heights, r, cols - 1, Integer.MIN_VALUE, atlantic);
        }
        
        List<List<Integer>> result = new ArrayList<>();
        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                if (pacific[r][c] && atlantic[r][c]) {
                    result.add(Arrays.asList(r, c));
                }
            }
        }
        
        return result;
    }
    
    private void dfs(int[][] heights, int r, int c, int prevHeight, boolean[][] ocean) {
        int rows = heights.length;
        int cols = heights[0].length;
        
        if (r < 0 || r >= rows || c < 0 || c >= cols || 
            ocean[r][c] || heights[r][c] < prevHeight) {
            return;
        }
        
        ocean[r][c] = true;
        
        int[][] dirs = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        for (int[] dir : dirs) {
            dfs(heights, r + dir[0], c + dir[1], heights[r][c], ocean);
        }
    }
}
```

---

## Intuition

The naive approach is to start from every cell and see if you can reach both oceans. However, this is redundant and slow.

### Reverse Thinking: Multi-Source DFS
Instead of starting from the land, **start from the oceans** and work backwards into the island.

1.  **Pacific Ocean**: Start from all cells in the top row and left column.
2.  **Atlantic Ocean**: Start from all cells in the bottom row and right column.
3.  **Flow Constraint**: Since we are moving from the ocean *up* into the island, water can only "flow" into a cell if its height is **greater than or equal** to the previous cell.

Cells that can be reached by both DFS traversals are the ones where water can flow to both oceans.

---

## Complexity Analysis

### Time Complexity

$O(M \times N)$

- $M$ is the number of rows, $N$ is the number of columns.
- Each cell is visited twice (once for each ocean).

### Space Complexity

$O(M \times N)$

- To store the two boolean grids.
- The recursion stack for DFS also goes up to $M \times N$ in the worst case.

---

## Key Takeaways

- Grid problems involving connectivity to boundaries are often solved efficiently using **multi-source traversal**.
- **Thinking in reverse** (from goal to start) can simplify constraints (changing "equal or lower" to "equal or higher").
- Space can be optimized, but using two separate boolean grids is the most readable and straightforward approach for this problem.
