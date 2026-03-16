---
title: 05 - Coin Change
tags:
  - Dynamic Programming
  - Optimization
---

# Coin Change Problem (Minimum Coins)

The Coin Change problem is a classic dynamic programming problem. Given a set of coin denominations and a target amount, find the minimum number of coins needed to make up that amount.

This is a variation of the **Unbounded Knapsack** problem, where you can use each item (coin) an infinite number of times.

## Problem
Given:
- `coins[]`: Denominations of available coins.
- `amount`: The target total.

Find the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

## 1. Recursive Approach
For each coin, we have the option to include it (and stay at the same index since it's unbounded) or skip it.

### Java Implementation
```java
public static int coinChange(int[] coins, int amount) {
    if (amount == 0) return 0;
    if (amount < 0) return Integer.MAX_VALUE;

    int minCoins = Integer.MAX_VALUE;
    for (int coin : coins) {
        int res = coinChange(coins, amount - coin);
        if (res != Integer.MAX_VALUE) {
            minCoins = Math.min(minCoins, 1 + res);
        }
    }
    return minCoins;
}
```
**Complexity**: Time $O(S^n)$ where $S$ is the amount and $n$ is the number of coins.

## 2. Memoization (Top-Down)
We use a 1D array `dp[amount + 1]` to store the minimum coins for each sub-amount.

### Java Implementation
```java
public static int coinChangeMemo(int[] coins, int amount, int[] dp) {
    if (amount == 0) return 0;
    if (amount < 0) return Integer.MAX_VALUE;
    if (dp[amount] != -1) return dp[amount];

    int minCoins = Integer.MAX_VALUE;
    for (int coin : coins) {
        int res = coinChangeMemo(coins, amount - coin, dp);
        if (res != Integer.MAX_VALUE) {
            minCoins = Math.min(minCoins, 1 + res);
        }
    }
    return dp[amount] = minCoins;
}
```
**Complexity**: Time $O(n \times \text{amount})$, Space $O(\text{amount})$.

## 3. Tabulation (Bottom-Up)
Iteratively fill the DP table from $1$ to `amount`.

### Java Implementation
```java
public static int coinChangeTab(int[] coins, int amount) {
    int max = amount + 1;
    int[] dp = new int[amount + 1];
    Arrays.fill(dp, max);
    dp[0] = 0;

    for (int i = 1; i <= amount; i++) {
        for (int coin : coins) {
            if (coin <= i) {
                dp[i] = Math.min(dp[i], dp[i - coin] + 1);
            }
        }
    }
    return dp[amount] > amount ? -1 : dp[amount];
}
```
**Complexity**: Time $O(n \times \text{amount})$, Space $O(\text{amount})$.

## Sample Input and Output

### Input
```text
coins = [1, 2, 5], amount = 11
```

### Output
```text
Min Coins: 3 (5 + 5 + 1)
```

## Key Takeaways
- **Greedy doesn't always work**: For denominations like `[1, 3, 4]` and amount `6`, Greedy gives `4 + 1 + 1 (3 coins)`, but DP gives `3 + 3 (2 coins)`.
- **Initialization**: Initialize the DP array with a value greater than `amount` (like `amount + 1`) to represent infinity.
- **Unbounded nature**: Unlike 0/1 Knapsack, we don't need to iterate backwards in space-optimized solutions because items can be reused.
