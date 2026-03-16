---
title: 04 - 0/1 Knapsack
tags:
  - Dynamic Programming
  - Optimization
---

# 0/1 Knapsack Problem

The 0/1 Knapsack problem is a foundational optimization problem. Given a set of items, each with a weight and a value, determine the number of each item to include in a collection so that the total weight is less than or equal to a given limit and the total value is as large as possible.

The "0/1" means you cannot break an item; you either take it ($1$) or leave it ($0$).

## Problem
Given:
- `weights[]`: Weights of $n$ items.
- `values[]`: Values of $n$ items.
- `W`: Maximum capacity of the knapsack.

Find the maximum total value that can be put in the knapsack.

## 1. Recursive Approach (Include/Exclude)
For each item, we have two choices:
1.  **Include** the item (if its weight $\le$ remaining capacity).
2.  **Exclude** the item.

### Java Implementation
```java
public static int knapsack(int[] wt, int[] val, int W, int n) {
    if (n == 0 || W == 0) return 0;

    if (wt[n - 1] <= W) {
        // Max(Include, Exclude)
        return Math.max(
            val[n - 1] + knapsack(wt, val, W - wt[n - 1], n - 1),
            knapsack(wt, val, W, n - 1)
        );
    } else {
        // Must exclude
        return knapsack(wt, val, W, n - 1);
    }
}
```
**Complexity**: Time $O(2^n)$, Space $O(n)$ (stack depth).

## 2. Memoization (Top-Down)
We use a 2D array `dp[n + 1][W + 1]` to store results of subproblems.

### Java Implementation
```java
public static int knapsackMemo(int[] wt, int[] val, int W, int n, int[][] dp) {
    if (n == 0 || W == 0) return 0;
    if (dp[n][W] != -1) return dp[n][W];

    if (wt[n - 1] <= W) {
        return dp[n][W] = Math.max(
            val[n - 1] + knapsackMemo(wt, val, W - wt[n - 1], n - 1, dp),
            knapsackMemo(wt, val, W, n - 1, dp)
        );
    } else {
        return dp[n][W] = knapsackMemo(wt, val, W, n - 1, dp);
    }
}
```
**Complexity**: Time $O(n \times W)$, Space $O(n \times W)$.

## 3. Tabulation (Bottom-Up)
Iteratively fill the DP table. `dp[i][w]` represents the maximum value using first `i` items with capacity `w`.

### Java Implementation
```java
public static int knapsackTab(int[] wt, int[] val, int W, int n) {
    int[][] dp = new int[n + 1][W + 1];

    for (int i = 1; i <= n; i++) {
        for (int w = 1; w <= W; w++) {
            if (wt[i - 1] <= w) {
                dp[i][w] = Math.max(val[i - 1] + dp[i - 1][w - wt[i - 1]], dp[i - 1][w]);
            } else {
                dp[i][w] = dp[i - 1][w];
            }
        }
    }
    return dp[n][W];
}
```

## 4. Space Optimized ($O(W)$)
Since we only need the previous row to calculate the current one, and we only look "left" (smaller `w`), we can use a single array and iterate **backwards**.

### Java Implementation
```java
public static int knapsackOptimized(int[] wt, int[] val, int W, int n) {
    int[] dp = new int[W + 1];

    for (int i = 0; i < n; i++) {
        // Iterate backwards to avoid using the same item multiple times
        for (int w = W; w >= wt[i]; w--) {
            dp[w] = Math.max(dp[w], val[i] + dp[w - wt[i]]);
        }
    }
    return dp[W];
}
```

## Sample Input and Output

### Input
```text
3 50
10 20 30
60 100 120
```
*(3 items, capacity 50; next lines are weights and values)*

### Output
```text
Max Value: 220
```

## Key Takeaways
- **0/1 Knapsack vs Fractional**: Use DP for 0/1 and Greedy for Fractional.
- **Backwards Iteration**: In the $O(W)$ optimization, iterating backwards ensures we don't count an item more than once (unlike the Unbounded Knapsack problem).
- **Complexity**: $O(n \times W)$ is "pseudo-polynomial" because it depends on the value of $W$.
