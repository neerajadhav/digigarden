---
title: 47 - House Robber
tags:
  - Dynamic Programming
  - Medium
---

You are given an integer array `nums` where `nums[i]` represents the amount of money the $i^{th}$ house has. The houses are arranged in a straight line, i.e. the $i^{th}$ house is the neighbor of the $(i-1)^{th}$ and $(i+1)^{th}$ house.

You are planning to rob money from the houses, but you cannot rob two adjacent houses because the security system will automatically alert the police if two adjacent houses were both broken into.

Return the maximum amount of money you can rob without alerting the police.

---

## Examples

**Example 1**

Input: `nums = [1,1,3,3]`  
Output: `4`  
Explanation: nums[0] + nums[2] = 1 + 3 = 4.

**Example 2**

Input: `nums = [2,9,8,3,6]`  
Output: `16`  
Explanation: nums[0] + nums[2] + nums[4] = 2 + 8 + 6 = 16.

---

## Solution

### Java Code

```java
class Solution {
    public int rob(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        if (nums.length == 1) {
            return nums[0];
        }

        int prev2 = 0; // f(i-2)
        int prev1 = 0; // f(i-1)

        for (int num : nums) {
            int current = Math.max(prev1, prev2 + num);
            prev2 = prev1;
            prev1 = current;
        }

        return prev1;
    }
}
```

---

## Intuition

For each house $i$, you have two choices:
1.  **Rob the house**: If you rob house $i$, you cannot rob house $i-1$. Your total profit would be `nums[i] + maxPossibleFrom(i-2)`.
2.  **Skip the house**: Your total profit would be the same as `maxPossibleFrom(i-1)`.

The recurrence relationship is:
$$f(i) = \max(f(i-1), f(i-2) + \text{nums}[i])$$

### Optimization
Similar to Climbing Stairs, we only need the last two states to calculate the current one. This allows us to reduce space complexity from $O(n)$ down to $O(1)$.

---

## Complexity Analysis

### Time Complexity

$O(n)$

- We iterate through the `nums` array exactly once.

### Space Complexity

$O(1)$

- We only use two variables (`prev1`, `prev2`) to store the previous results.

---

## Key Takeaways

- The "House Robber" problem is a classic example of **Dynamic Programming** where the decision at step $i$ depends on calculations from previous steps.
- The constraint of "no adjacent elements" is a common pattern that usually points toward DP.
- Base cases (0 or 1 house) must be handled explicitly.
