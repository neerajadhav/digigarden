---
title: 53 - Maximum Product Subarray
tags:
  - Dynamic Programming
  - Array
  - Medium
---

Given an integer array `nums`, find a subarray by tracking both minimum and maximum products that has the largest product within the array and return it.

A subarray is a contiguous non-empty sequence of elements within an array.

---

## Examples

**Example 1**

Input: `nums = [1,2,-3,4]`  
Output: `4`

**Example 2**

Input: `nums = [-2,-1]`  
Output: `2`

---

## Solution

### Java Code

```java
class Solution {
    public int maxProduct(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }

        int maxSoFar = nums[0];
        int minSoFar = nums[0];
        int result = maxSoFar;

        for (int i = 1; i < nums.length; i++) {
            int current = nums[i];
            
            // If current is negative, max and min will swap after multiplication
            if (current < 0) {
                int temp = maxSoFar;
                maxSoFar = minSoFar;
                minSoFar = temp;
            }

            // At each step, we decide whether to start a new subarray or extend the current one
            maxSoFar = Math.max(current, maxSoFar * current);
            minSoFar = Math.min(current, minSoFar * current);

            result = Math.max(result, maxSoFar);
        }

        return result;
    }
}
```

---

## Intuition

This problem is similar to **Kadane's Algorithm** (Maximum Sum Subarray), but with a twist: **Products can be negative**.

### Why track "Min"?
Multiplying a very small negative number by another negative number can suddenly create a very large positive number. Therefore, at each step, we need to track both:
1.  The maximum product ending at the current position.
2.  The minimum product ending at the current position (in case it becomes positive later).

### State Transitions
When we encounter a negative number, the `maxSoFar` and `minSoFar` swap their potential roles. Multiplying the `max` by a negative makes it a `min`, and multiplying the `min` by a negative makes it a `max`.

---

## Complexity Analysis

### Time Complexity

$O(n)$

- We traverse the array exactly once.

### Space Complexity

$O(1)$

- We only use a few variables (`maxSoFar`, `minSoFar`, `result`) to keep track of state.

---

## Key Takeaways

- Negative numbers "flip" the sign of products, making double-tracking (max and min) necessary.
- Kadane-style DP is extremely efficient for subarray problems.
- Zeros act as "resets" in this algorithm because they nullify any previous products.
