---
title: 58 - Maximum Subarray
tags:
  - Greedy
  - Array
  - Medium
---

Given an array of integers `nums`, find the subarray with the largest sum and return the sum.

A subarray is a contiguous non-empty sequence of elements within an array.

---

## Examples

**Example 1**

Input: `nums = [2,-3,4,-2,2,1,-1,4]`  
Output: `8`  
Explanation: The subarray `[4,-2,2,1,-1,4]` has the largest sum 8.

**Example 2**

Input: `nums = [-1]`  
Output: `-1`

---

## Solution

### Java Code

```java
class Solution {
    public int maxSubArray(int[] nums) {
        // Kadane's Algorithm
        int maxSoFar = nums[0];
        int currentMax = nums[0];

        for (int i = 1; i < nums.length; i++) {
            // At each step, decide whether to:
            // 1. Extend the existing subarray (currentMax + nums[i])
            // 2. Start a new subarray starting from current element (nums[i])
            currentMax = Math.max(nums[i], currentMax + nums[i]);
            
            // Update the global maximum found so far
            maxSoFar = Math.max(maxSoFar, currentMax);
        }

        return maxSoFar;
    }
}
```

---

## Intuition

The problem asks for the maximum sum of a **contiguous** subarray. The most efficient way to solve this is using **Kadane's Algorithm**.

### Greedy Thinking
At any index $i$, we have two choices for the local maximum sum ending at $i$:
1.  **Extend**: If the previous sum was positive, adding the current element will likely help (or at least be the best we can do ending here).
2.  **Reset**: If the previous sum was negative, it's better to discard it and start a new subarray beginning exactly at the current element.

Mathematically:
$$\text{currentMax}_i = \max(\text{nums}[i], \text{currentMax}_{i-1} + \text{nums}[i])$$

---

## Complexity Analysis

### Time Complexity

$O(n)$

- We traverse the array exactly once.

### Space Complexity

$O(1)$

- We only use two variables to track the maximum sums.

---

## Key Takeaways

- Kadane's algorithm is the gold standard for Maximum Subarray Sum.
- It works by making a locally optimal choice (extend or reset) at each step to ensure a globally optimal result.
- This is a classic example of how a problem that looks like it might need $O(n^2)$ can be reduced to $O(n)$ with the right greedy intuition.
