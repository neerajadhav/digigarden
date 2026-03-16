---
title: 06 - Matrix Chain Multiplication
tags:
  - Dynamic Programming
  - Partitioning
---

# Matrix Chain Multiplication (MCM)

Matrix Chain Multiplication is a classic problem that demonstrates the **Partitioning** pattern in Dynamic Programming. Given a sequence of matrices, the goal is to find the most efficient way to multiply them together.

The problem is not actually to perform the multiplications, but merely to decide the order of multiplications that minimizes the number of scalar multiplications.

## Problem
Given an array `arr[]` which represents the dimensions of $n$ matrices such that the $i$-th matrix $A_i$ has dimensions `arr[i-1] x arr[i]`.

Find the minimum number of scalar multiplications needed to multiply the chain of $n$ matrices.

## 1. Recursive Approach
The idea is to place a parenthesis at all possible places, calculate the cost recursively, and take the minimum.

### Java Implementation
```java
public static int solve(int[] arr, int i, int j) {
    if (i >= j) return 0;

    int minCost = Integer.MAX_VALUE;

    // Partition the range [i, j] at every possible index k
    for (int k = i; k < j; k++) {
        int tempCost = solve(arr, i, k) + solve(arr, k + 1, j) 
                       + (arr[i - 1] * arr[k] * arr[j]);
        
        minCost = Math.min(minCost, tempCost);
    }

    return minCost;
}
```
**Complexity**: Time $O(2^n)$ (Exponential), Space $O(n)$.

## 2. Memoization (Top-Down)
We use a 2D array `dp[n][n]` to store the minimum cost for the range `[i, j]`.

### Java Implementation
```java
public static int solveMemo(int[] arr, int i, int j, int[][] dp) {
    if (i >= j) return 0;
    if (dp[i][j] != -1) return dp[i][j];

    int minCost = Integer.MAX_VALUE;

    for (int k = i; k < j; k++) {
        int tempCost = solveMemo(arr, i, k, dp) + solveMemo(arr, k + 1, j, dp) 
                       + (arr[i - 1] * arr[k] * arr[j]);
        
        minCost = Math.min(minCost, tempCost);
    }

    return dp[i][j] = minCost;
}
```
**Complexity**: Time $O(n^3)$, Space $O(n^2)$.

## 3. Tabulation (Bottom-Up)
Iteratively fill the DP table based on the length of the chain.

### Java Implementation
```java
public static int solveTab(int[] arr, int n) {
    int[][] dp = new int[n][n];

    // l is chain length
    for (int l = 2; l < n; l++) {
        for (int i = 1; i < n - l + 1; i++) {
            int j = i + l - 1;
            dp[i][j] = Integer.MAX_VALUE;
            for (int k = i; k <= j - 1; k++) {
                int q = dp[i][k] + dp[k + 1][j] + arr[i - 1] * arr[k] * arr[j];
                if (q < dp[i][j]) dp[i][j] = q;
            }
        }
    }

    return dp[1][n - 1];
}
```

## Sample Input and Output

### Input
```text
arr = [10, 20, 30, 40, 30]
```
*(Matrices: 10x20, 20x30, 30x40, 40x30)*

### Output
```text
Minimum Cost: 30000
```

## Key Takeaways
- **Partitioning Pattern**: This approach is used in many problems like "Minimum Cost to Cut a Stick" or "Burst Balloons".
- **Complexity**: The $O(n^3)$ time complexity arises from $n^2$ states and $O(n)$ work per state (the loop over $k$).
- **Base Case**: The base case `i >= j` represents a single matrix (or invalid range), which requires 0 multiplications.
