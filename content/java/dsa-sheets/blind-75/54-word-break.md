---
title: 54 - Word Break
tags:
  - Dynamic Programming
  - Hash Table
  - Medium
---

Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of dictionary words.

You are allowed to reuse words in the dictionary an unlimited number of times. You may assume all dictionary words are unique.

---

## Examples

**Example 1**

Input: `s = "neetcode"`, `wordDict = ["neet","code"]`  
Output: `true`  
Explanation: Return `true` because "neetcode" can be split into "neet" and "code".

**Example 2**

Input: `s = "applepenapple"`, `wordDict = ["apple","pen","ape"]`  
Output: `true`  
Explanation: Return `true` because "applepenapple" can be split into "apple", "pen" and "apple".

**Example 3**

Input: `s = "catsincars"`, `wordDict = ["cats","cat","sin","in","car"]`  
Output: `false`

---

## Solution

### Java Code

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        // Use a set for O(1) word lookups
        Set<String> wordSet = new HashSet<>(wordDict);
        
        // dp[i] is true if the substring s[0...i-1] can be segmented
        boolean[] dp = new boolean[s.length() + 1];
        dp[0] = true; // Base case: empty string can be segmented

        for (int i = 1; i <= s.length(); i++) {
            for (int j = 0; j < i; j++) {
                // If s[0...j-1] can be segmented and s[j...i-1] is in dictionary
                if (dp[j] && wordSet.contains(s.substring(j, i))) {
                    dp[i] = true;
                    break; // No need to check other j for this i
                }
            }
        }

        return dp[s.length()];
    }
}
```

---

## Intuition

We want to know if the entire string `s` can be broken into words. This depends on whether smaller prefixes of `s` can be broken into words.

### Dynamic Programming
Let `dp[i]` be a boolean representing whether the substring `s[0...i-1]` is validly segmentable.
To find if `dp[i]` is true, we look back at all possible split points `j < i`:
- If `dp[j]` is true (prefix `s[0...j-1]` is valid) AND
- The substring `s[j...i-1]` exists in the dictionary.

Then `dp[i]` becomes true.

### Optimization
Using a `HashSet` for the dictionary reduces the word lookup time from $O(n)$ to $O(1)$ on average.

---

## Complexity Analysis

### Time Complexity

$O(n^2 \cdot m)$ or $O(n^3)$

- We have two nested loops over the length of the string $n$ ($O(n^2)$).
- Inside the loop, `s.substring(j, i)` and `wordSet.contains()` take $O(n)$ in the worst case (string length).

### Space Complexity

$O(n + d)$

- $O(n)$ for the `dp` array.
- $O(d)$ for the `HashSet` storing the dictionary words.

---

## Key Takeaways

- Word break is a classical "String DP" problem where the state depends on sub-prefixes.
- Always use a `Set` for dictionary problems to ensure efficient lookups.
- The `substring` method in Java creates a new string object, contributing to the complexity.
