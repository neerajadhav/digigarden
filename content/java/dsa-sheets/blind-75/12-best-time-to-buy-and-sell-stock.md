---
title: 12 - Best Time to Buy and Sell Stock
tags:
  - Array
  - Sliding-Window
  - Easy
---

You are given an integer array `prices` where `prices[i]` is the price of NeetCoin on the $i^{th}$ day.

You may choose a single day to buy one NeetCoin and choose a different day in the future to sell it.

Return the **maximum profit** you can achieve. You may choose to not make any transactions, in which case the profit would be 0.

---

## Examples

**Example 1**

Input: 
```
prices = [10, 1, 5, 6, 7, 1]
```

Output: 
```
6
```

Explanation: Buy `prices[1]` (price = 1) and sell `prices[4]` (price = 7), profit = 7 - 1 = 6.

---

**Example 2**

Input: 
```
prices = [10, 8, 7, 5, 2]
```

Output: 
```
0
```

Explanation: No profitable transactions can be made, thus the max profit is 0.

---

## Solution

### Java Code

```java
class Solution {
    public int maxProfit(int[] prices) {
        int minPrice = Integer.MAX_VALUE;
        int maxProfit = 0;

        for (int price : prices) {
            // Update the minimum price seen so far
            minPrice = Math.min(minPrice, price);
            
            // Calculate potential profit if sold today and update maxProfit
            maxProfit = Math.max(maxProfit, price - minPrice);
        }

        return maxProfit;
    }
}
```

---

## Java Utility Components Used

## 1. `Math.min()` & `Math.max()`

### What they are
Static utility methods from the `java.lang.Math` class used to find the minimum and maximum of numerical values.

### How they are used
```java
minPrice = Math.min(minPrice, price);
maxProfit = Math.max(maxProfit, price - minPrice);
```
In this "Sliding Window" / "One Pass" approach, these methods keep our loop body extremely concise by handling the comparison logic cleanly.

---

## 2. `Integer.MAX_VALUE`
Used to initialize `minPrice` with the largest possible integer value to ensure any price from the array will be smaller than the initial value.

---

## Intuition

The goal is to find the maximum difference between two prices where the smaller price comes before the larger price in the array.

1. **One Pass Strategy**: As we iterate through the list, we keep track of the **lowest price seen so far** (`minPrice`).
2. **Current Profit**: For every price we encounter, we calculate the profit we would make if we bought at `minPrice` and sold at the current `price`.
3. **Capture Max**: We update our `maxProfit` if this current profit is greater than any we've seen before.

This is a simplified version of the **Sliding Window** technique where the "window" expands to include the current day, but the "buying day" only shifts when we find a new historical low.

---

## Complexity Analysis

### Time Complexity

```
O(n)
```

We traverse the array exactly once.

---

### Space Complexity

```
O(1)
```

Only two integer variables (`minPrice` and `maxProfit`) are used regardless of the input size.

---

## Key Takeaways

- **One-pass optimization** avoids the $O(n^2)$ complexity of nested loops.
- Tracking **historical minimums** is a common pattern for "best time" or "maximum difference" problems.
- `Math` utilities make the logic more declarative and less error-prone than manual `if` checks.
