---
title: 55 - Longest Increasing Subsequence
tags:
  - Dynamic Programming
  - Array
  - Medium
---

Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

A subsequence is a sequence that can be derived from the given sequence by deleting some or no elements without changing the relative order of the remaining characters.

---

## Examples

**Example 1**

Input: `nums = [9,1,4,2,3,3,7]`  
Output: `4`  
Explanation: The longest increasing subsequence is [1,2,3,7], which has a length of 4.

**Example 2**

Input: `nums = [0,3,1,3,2,3]`  
Output: `4`

---

## Solution

### Java Code

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }

        int n = nums.length;
        // dp[i] stores the length of the LIS ending at index i
        int[] dp = new int[n];
        // Every single element is an increasing subsequence of length 1
        Arrays.fill(dp, 1);
        
        int maxLen = 1;

        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                // If the current element is greater than a previous element
                if (nums[i] > nums[j]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            maxLen = Math.max(maxLen, dp[i]);
        }

        return maxLen;
    }
}
```

---

## Intuition

To find the longest increasing subsequence ending at index `i`, we look at all previous indices `j < i`. If `nums[i] > nums[j]`, then `nums[i]` can extend the increasing subsequence that ends at `j`.

### Dynamic Programming
Let `dp[i]` be the length of the longest strictly increasing subsequence that ends with the element `nums[i]`.

The recurrence relation is:
$$dp[i] = 1 + \max(\{dp[j] \mid 0 \le j < i \text{ and } \text{nums}[j] < \text{nums}[i]\} \cup \{0\})$$

### Overall Result
The answer to the problem is the maximum value found in the `dp` array.

---

## Complexity Analysis

### Time Complexity

$O(n^2)$

- We have two nested loops. The outer loop runs $n$ times, and the inner loop runs up to $n$ times for each outer iteration.

### Space Complexity

$O(n)$

- We use a `dp` array of size $n$ to store the length of the LIS ending at each index.

---

## Key Takeaways

- LIS is a classic DP problem that demonstrates how a subproblem's value depends on results from multiple prior indices.
- While $O(n^2)$ is the standard DP approach, there is an optimized $O(n \log n)$ solution using **Binary Search** and a "tails" array.
- A "subsequence" is different from a "substring" as it does not require elements to be contiguous.
