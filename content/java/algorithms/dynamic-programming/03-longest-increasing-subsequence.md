---
title: 03 - Longest Increasing Subsequence
tags:
  - Dynamic Programming
  - Binary Search
  - Arrays
---

# Longest Increasing Subsequence (LIS)

The Longest Increasing Subsequence (LIS) problem is a classic dynamic programming problem where we find the length of the longest subsequence such that all elements are sorted in increasing order.

A **subsequence** is derived by deleting zero or more elements from the original array without changing the order of the remaining elements.

## Problem
Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

## 1. Recursive Approach
For each element, we have two choices:
1.  **Include** it if it's greater than the previously included element.
2.  **Exclude** it and move to the next.

### Java Implementation
```java
public static int lcs(int[] nums, int prev_idx, int curr_idx) {
    if (curr_idx == nums.length) return 0;

    int include = 0;
    if (prev_idx == -1 || nums[curr_idx] > nums[prev_idx]) {
        include = 1 + lcs(nums, curr_idx, curr_idx + 1);
    }
    int exclude = lcs(nums, prev_idx, curr_idx + 1);

    return Math.max(include, exclude);
}
```
**Complexity**: Time $O(2^n)$, Space $O(n)$.

## 2. Memoization (Top-Down)
We use a 2D array `dp[nums.length][nums.length + 1]` to store results where the second dimension represents the index of the previous element (+1 for mapping -1 to 0).

### Java Implementation
```java
public static int lcsMemo(int[] nums, int prev_idx, int curr_idx, int[][] dp) {
    if (curr_idx == nums.length) return 0;
    if (dp[curr_idx][prev_idx + 1] != -1) return dp[curr_idx][prev_idx + 1];

    int include = 0;
    if (prev_idx == -1 || nums[curr_idx] > nums[prev_idx]) {
        include = 1 + lcsMemo(nums, curr_idx, curr_idx + 1, dp);
    }
    int exclude = lcsMemo(nums, prev_idx, curr_idx + 1, dp);

    return dp[curr_idx][prev_idx + 1] = Math.max(include, exclude);
}
```
**Complexity**: Time $O(n^2)$, Space $O(n^2)$.

## 3. Tabulation (Bottom-Up)
A more common $O(n^2)$ approach is using a 1D DP array where `dp[i]` stores the LIS ending at index `i`.

### Java Implementation
```java
public static int lcsTab(int[] nums) {
    int n = nums.length;
    int[] dp = new int[n];
    Arrays.fill(dp, 1);
    int maxLIS = 1;

    for (int i = 1; i < n; i++) {
        for (int j = 0; j < i; j++) {
            if (nums[i] > nums[j]) {
                dp[i] = Math.max(dp[i], 1 + dp[j]);
            }
        }
        maxLIS = Math.max(maxLIS, dp[i]);
    }
    return maxLIS;
}
```

## 4. Binary Search Optimization ($O(n \log n)$)
We maintain a "tails" list where `tails[i]` is the smallest tail of all increasing subsequences of length `i+1`.

### Java Implementation
```java
public static int lcsOptimal(int[] nums) {
    int n = nums.length;
    List<Integer> tails = new ArrayList<>();

    for (int num : nums) {
        int idx = Collections.binarySearch(tails, num);
        if (idx < 0) idx = -(idx + 1);

        if (idx < tails.size()) {
            tails.set(idx, num);
        } else {
            tails.add(num);
        }
    }
    return tails.size();
}
```

## 5. Reconstructing the LIS Sequence
To get the actual sequence, we use a `parent` array to track which element preceded the current one in the $O(n^2)$ approach.

### Java Implementation
```java
public static List<Integer> getLIS(int[] nums) {
    int n = nums.length;
    int[] dp = new int[n];
    int[] parent = new int[n];
    Arrays.fill(dp, 1);
    Arrays.fill(parent, -1);
    
    int lastIdx = 0;
    for (int i = 1; i < n; i++) {
        for (int j = 0; j < i; j++) {
            if (nums[i] > nums[j] && 1 + dp[j] > dp[i]) {
                dp[i] = 1 + dp[j];
                parent[i] = j;
            }
        }
        if (dp[i] > dp[lastIdx]) lastIdx = i;
    }

    List<Integer> result = new ArrayList<>();
    while (lastIdx != -1) {
        result.add(nums[lastIdx]);
        lastIdx = parent[lastIdx];
    }
    Collections.reverse(result);
    return result;
}
```

## Sample Input and Output

### Input
```text
6
10 9 2 5 3 7 101 18
```

### Output
```text
LIS Length: 4
LIS Sequence: [2, 3, 7, 18]
```

## Key Takeaways
- The $O(n^2)$ approach is intuitive and easy to adapt (e.g., finding the longest *non-decreasing* subsequence).
- The $O(n \log n)$ approach is much faster for large inputs but only gives the length (not necessarily the correct lexicographical subsequence directly in the `tails` array).
- Reconstructing the sequence requires an auxiliary array to store the "path".
