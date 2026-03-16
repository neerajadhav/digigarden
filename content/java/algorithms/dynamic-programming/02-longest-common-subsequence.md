---
title: 02 - Longest Common Subsequence
tags:
  - Dynamic Programming
  - Strings
---

# Longest Common Subsequence (LCS)

The Longest Common Subsequence (LCS) problem is a classic dynamic programming problem. Given two strings, find the length of the longest subsequence present in both of them.

A **subsequence** is a sequence that appears in the same relative order, but not necessarily contiguously.

## Problem
Given two strings `text1` and `text2`, return the length of their longest common subsequence.

## 1. Recursive Approach
At each step, we compare the current characters:
- If they match: $1 + LCS(\text{rest of both})$
- If they don't match: $\max(LCS(\text{rest of } S1, S2), LCS(S1, \text{rest of } S2))$

### Java Implementation
```java
public static int lcs(String s1, String s2, int m, int n) {
    if (m == 0 || n == 0) return 0;
    if (s1.charAt(m - 1) == s2.charAt(n - 1))
        return 1 + lcs(s1, s2, m - 1, n - 1);
    else
        return Math.max(lcs(s1, s2, m, n - 1), lcs(s1, s2, m - 1, n));
}
```
**Complexity**: Time $O(2^{\max(m, n)})$, Space $O(\max(m, n))$ (stack depth).

## 2. Memoization (Top-Down)
We use a 2D array to store the results of subproblems.

### Java Implementation
```java
public static int lcsMemo(String s1, String s2, int m, int n, int[][] dp) {
    if (m == 0 || n == 0) return 0;
    if (dp[m][n] != -1) return dp[m][n];

    if (s1.charAt(m - 1) == s2.charAt(n - 1))
        return dp[m][n] = 1 + lcsMemo(s1, s2, m - 1, n - 1, dp);
    else
        return dp[m][n] = Math.max(lcsMemo(s1, s2, m, n - 1, dp), lcsMemo(s1, s2, m - 1, n, dp));
}
```
**Complexity**: Time $O(m \times n)$, Space $O(m \times n)$.

## 3. Tabulation (Bottom-Up)
Iteratively fill the DP table.

### Java Implementation
```java
public static int lcsTab(String s1, String s2) {
    int m = s1.length(), n = s2.length();
    int[][] dp = new int[m + 1][n + 1];

    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (s1.charAt(i - 1) == s2.charAt(j - 1))
                dp[i][j] = 1 + dp[i - 1][j - 1];
            else
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
        }
    }
    return dp[m][n];
}
```

## 4. Reconstructing the LCS String
To find the actual string, we backtrack through the populated DP table.

### Java Implementation
```java
public static String getLCSString(String s1, String s2, int[][] dp) {
    StringBuilder sb = new StringBuilder();
    int i = s1.length(), j = s2.length();
    while (i > 0 && j > 0) {
        if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
            sb.append(s1.charAt(i - 1));
            i--; j--;
        } else if (dp[i - 1][j] > dp[i][j - 1]) {
            i--;
        } else {
            j--;
        }
    }
    return sb.reverse().toString();
}
```

## 5. Space Optimized ($O(n)$)
Since we only need the previous row to calculate the current one, we can reduce space.

### Java Implementation
```java
public static int lcsOptimized(String s1, String s2) {
    int m = s1.length(), n = s2.length();
    int[] prev = new int[n + 1];
    int[] curr = new int[n + 1];

    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (s1.charAt(i - 1) == s2.charAt(j - 1))
                curr[j] = 1 + prev[j - 1];
            else
                curr[j] = Math.max(prev[j], curr[j - 1]);
        }
        prev = curr.clone();
    }
    return prev[n];
}
```

## Sample Input and Output

### Input
```text
abcde
ace
```

### Output
```text
LCS Length: 3
LCS String: ace
```

## Key Takeaways
- LCS is the foundation for other problems like Longest Palindromic Subsequence and Edit Distance.
- **Space Optimization** is crucial when strings are very long.
- Backtracking allows us to extract the actual sequence from the DP state.
