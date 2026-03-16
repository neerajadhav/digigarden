---
title: 52 - Coin Change
tags:
  - Dynamic Programming
  - Medium
---

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a target amount of money.

Return the fewest number of coins that you need to make up the exact target amount. If it is impossible to make up the amount, return -1.

You may assume that you have an unlimited number of each coin.

---

## Examples

**Example 1**

Input: `coins = [1,5,10]`, `amount = 12`  
Output: `3`  
Explanation: 12 = 10 + 1 + 1.

**Example 2**

Input: `coins = [2]`, `amount = 3`  
Output: `-1`

**Example 3**

Input: `coins = [1]`, `amount = 0`  
Output: `0`

---

## Solution

### Java Code

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        if (amount < 1) return 0;
        
        // dp[i] will store the minimum coins needed for amount i
        int[] dp = new int[amount + 1];
        
        // Initialize with a value larger than any possible answer (amount + 1)
        Arrays.fill(dp, amount + 1);
        dp[0] = 0;
        
        for (int i = 1; i <= amount; i++) {
            for (int coin : coins) {
                if (i >= coin) {
                    dp[i] = Math.min(dp[i], dp[i - coin] + 1);
                }
            }
        }
        
        return dp[amount] > amount ? -1 : dp[amount];
    }
}
```

---

## Intuition

This is a classic **Unbounded Knapsack** variant. For each possible amount from 1 to `target`, we want to find the minimum number of coins required.

### Dynamic Programming Approach
The state `dp[i]` represents the minimum coins needed to make amount `i`. To calculate `dp[i]`, we look at every coin available:
- If we use a coin of denomination `c`, the remaining amount is `i - c`.
- The number of coins used would be `1 + dp[i - c]`.
- We take the minimum across all possible coins.

Recurrence:
$$dp[i] = \min_{c \in \text{coins}} (dp[i - c] + 1) \text{ where } i \ge c$$

---

## Complexity Analysis

### Time Complexity

$O(\text{amount} \cdot n)$

- We have a nested loop: the outer loop runs `amount` times, and the inner loop runs for each of the $n$ coin denominations.

### Space Complexity

$O(\text{amount})$

- We store a `dp` array of size `amount + 1`.

---

## Key Takeaways

- Initialize DP arrays for "minimum" problems with a large value (like `amount + 1` or `Infinity`) rather than `Integer.MAX_VALUE` to avoid overflow when adding 1.
- This problem is significantly different from the "Greedy" approach (which doesn't always work for arbitrary denominations).
- The "Unbounded" nature of the problem is handled by the order of iteration in the DP table.
