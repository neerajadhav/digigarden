---
title: 48 - House Robber II
tags:
  - Dynamic Programming
  - Medium
---

You are given an integer array `nums` where `nums[i]` represents the amount of money the $i^{th}$ house has. The houses are arranged in a circle, i.e. the first house and the last house are neighbors.

You are planning to rob money from the houses, but you cannot rob two adjacent houses because the security system will automatically alert the police if two adjacent houses were both broken into.

Return the maximum amount of money you can rob without alerting the police.

---

## Examples

**Example 1**

Input: `nums = [3,4,3]`  
Output: `4`  
Explanation: You cannot rob `nums[0] + `nums[2]` = 6 because `nums[0]` and `nums[2]` are adjacent houses. The maximum you can rob is `nums[1] = 4`.

**Example 2**

Input: `nums = [2,9,8,3,6]`  
Output: `15`  
Explanation: You cannot rob `nums[0] + nums[2] + nums[4]` = 16 because `nums[0]` and `nums[4]` are adjacent houses. The maximum you can rob is `nums[1] + nums[4] = 15`.

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

        // To handle the circular constraint, we solve two sub-problems:
        // 1. Rob houses from index 0 to n-2 (exclude the last house)
        // 2. Rob houses from index 1 to n-1 (exclude the first house)
        // The result is the maximum of these two.
        return Math.max(robRange(nums, 0, nums.length - 2), 
                        robRange(nums, 1, nums.length - 1));
    }

    private int robRange(int[] nums, int start, int end) {
        int prev2 = 0;
        int prev1 = 0;

        for (int i = start; i <= end; i++) {
            int current = Math.max(prev1, prev2 + nums[i]);
            prev2 = prev1;
            prev1 = current;
        }

        return prev1;
    }
}
```

---

## Intuition

The "circular" arrangement is the primary challenge. Because the first and last houses are neighbors, you cannot rob both.

This constraint means we have to make a choice:
1.  **Skip the last house**: Rob optimally from house $0$ to $n-2$.
2.  **Skip the first house**: Rob optimally from house $1$ to $n-1$.

By splitting the problem into these two linear ranges, we can reuse the optimized solution from **House Robber I**. The overall maximum will be the higher value between these two scenarios.

---

## Complexity Analysis

### Time Complexity

$O(n)$

- We run the linear "House Robber" algorithm twice: once for $n-1$ houses and once for another $n-1$ houses.
- Total operations are $2 \cdot O(n)$, which simplifies to $O(n)$.

### Space Complexity

$O(1)$

- We only use a few constant variables (`prev1`, `prev2`) during the calculation.

---

## Key Takeaways

- **Circular DP** problems can often be broken down into multiple **Linear DP** sub-problems.
- Reusing existing logic (like `robRange`) keeps the code clean and maintainable.
- Handling the base case for an array of length 1 is critical when slicing indices.
