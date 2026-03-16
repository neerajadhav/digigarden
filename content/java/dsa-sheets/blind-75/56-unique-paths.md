---
title: 56 - Unique Paths
tags:
  - Dynamic Programming
  - Matrix
  - Medium
---

There is an $m \times n$ grid where you are allowed to move either **down** or to the **right** at any point in time.

Given the two integers $m$ and $n$, return the number of possible unique paths that can be taken from the top-left corner of the grid (`grid[0][0]`) to the bottom-right corner (`grid[m - 1][n - 1]`).

---

## Examples

**Example 1**

Input: `m = 3`, `n = 7`  
Output: `28`

![Unique Paths](../../../assets/attachments/count-paths-1.avif)

**Example 2**

Input: `m = 3`, `n = 2`  
Output: `3`  
Explanation: From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Down -> Down
2. Down -> Down -> Right
3. Down -> Right -> Down

---

## Solution

### Java Code

```java
class Solution {
    public int uniquePaths(int m, int n) {
        // We only need one row to store the DP states
        int[] dp = new int[n];
        
        // Initialize the first row (only 1 way to reach any cell in the first row: keep moving right)
        for (int j = 0; j < n; j++) {
            dp[j] = 1;
        }

        // Iterate through each remaining row
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                // The number of paths to current cell is:
                // paths from above (current dp[j]) + paths from left (current dp[j-1])
                dp[j] += dp[j - 1];
            }
        }

        return dp[n - 1];
    }
}
```

---

## Intuition

This is a classic grid-based DP problem. Since you can only move **Right** or **Down**, the number of ways to reach a cell $(i, j)$ is the sum of ways to reach $(i-1, j)$ and $(i, j-1)$.

### Dynamic Programming
Let `dp[i][j]` be the number of unique paths to reach cell $(i, j)$.
The recurrence relation is:
$$dp[i][j] = dp[i-1][j] + dp[i][j-1]$$

### Space Optimization
Notice that to calculate the current row, we only need the values from the same column in the row immediately above (`dp[i-1][j]`) and the value from the current row but the previous column (`dp[i][j-1]`).
This allows us to collapse the 2D matrix into a 1D array of size $n$.

---

## Complexity Analysis

### Time Complexity

$O(m \cdot n)$

- We iterate through every cell in the $m \times n$ grid once.

### Space Complexity

$O(n)$

- Thanks to space optimization, we only store a single row of size $n$.

---

## Key Takeaways

- Grid problems with restricted movement (Right/Down) are a perfect fit for **DP**.
- The base case is the first row and first column, which only have 1 path each.
- This problem can also be solved using **Combinatorics** (nCr) since it's equivalent to choosing $(m-1)$ downs out of $(m+n-2)$ total moves.
