---
title: 57 - Longest Common Subsequence
tags:
  - Dynamic Programming
  - String
  - Medium
---

Given two strings `text1` and `text2`, return the length of the longest common subsequence between the two strings if one exists, otherwise return 0.

A subsequence is a sequence that can be derived from the given sequence by deleting some or no elements without changing the relative order of the remaining characters.

---

## Examples

**Example 1**

Input: `text1 = "cat", text2 = "crabt"`  
Output: `3`  
Explanation: The longest common subsequence is "cat" which has a length of 3.

**Example 2**

Input: `text1 = "abcd", text2 = "abcd"`  
Output: `4`

**Example 3**

Input: `text1 = "abcd", text2 = "efgh"`  
Output: `0`

---

## Solution

### Java Code

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length();
        int n = text2.length();
        
        // dp[i][j] stores the LCS length for text1[0...i-1] and text2[0...j-1]
        int[][] dp = new int[m + 1][n + 1];
        
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                // If characters match, add 1 to the result of prefixes
                if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                } else {
                    // Otherwise, take the maximum from excluding either character
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        
        return dp[m][n];
    }
}
```

---

## Intuition

This is a classic **Two-String Dynamic Programming** problem. We compare characters of both strings and build the solution based on whether they match.

### Dynamic Programming
Let `dp[i][j]` be the length of the longest common subsequence of `text1[0...i-1]` and `text2[0...j-1]`.

1.  **If `text1[i-1] == text2[j-1]`**: The last characters match, so they must be part of the LCS.
    $$dp[i][j] = 1 + dp[i-1][j-1]$$
2.  **If `text1[i-1] != text2[j-1]`**: The last characters don't match. The LCS could potentially end with a character from either the first string's prefix or the second string's prefix.
    $$dp[i][j] = \max(dp[i-1][j], dp[i][j-1])$$

### Space Optimization
Note that to calculate the current row, we only need the current and the previous row. This can be optimized to $O(\min(m, n))$ space by using only two rows.

---

## Complexity Analysis

### Time Complexity

$O(m \cdot n)$

- We fill a 2D table of size $(m+1) \times (n+1)$, performing a constant-time operation for each cell.

### Space Complexity

$O(m \cdot n)$

- We store a 2D array of size $(m+1) \times (n+1)$.

---

## Key Takeaways

- LCS is a foundational problem for many string comparison algorithms (like `diff`).
- The base case is when either string is empty, resulting in an LCS of length 0.
- This pattern (match vs. no-match) is common in many string DP problems like Edit Distance or Longest Common Substring.
