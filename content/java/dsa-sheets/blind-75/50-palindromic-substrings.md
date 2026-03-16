---
title: 50 - Palindromic Substrings
tags:
  - Dynamic Programming
  - String
  - Two Pointers
  - Medium
---

Given a string `s`, return the number of substrings within `s` that are palindromes.

A palindrome is a string that reads the same forward and backward.

---

## Examples

**Example 1**

Input: `s = "abc"`  
Output: `3`  
Explanation: "a", "b", "c".

**Example 2**

Input: `s = "aaa"`  
Output: `6`  
Explanation: "a", "a", "a", "aa", "aa", "aaa". Note that different substrings are counted as different palindromes even if the string contents are the same.

---

## Solution

### Java Code

```java
class Solution {
    public int countSubstrings(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }

        int count = 0;
        for (int i = 0; i < s.length(); i++) {
            // Case 1: Count palindromes with odd length (centered at i)
            count += expandAndCount(s, i, i);
            // Case 2: Count palindromes with even length (centered between i and i+1)
            count += expandAndCount(s, i, i + 1);
        }

        return count;
    }

    private int expandAndCount(String s, int left, int right) {
        int count = 0;
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            count++;
            left--;
            right++;
        }
        return count;
    }
}
```

---

## Intuition

A palindrome is symmetric around its center. To count all palindromic substrings, we can iterate through every possible center and expand outwards as long as the characters match.

### Expand Around Center Approach
1.  **Possible Centers**: For a string of length $n$, there are $2n - 1$ possible centers (each character and each gap between characters).
2.  **Expansion**: For each center, we expand to the left and right. Each time the characters match, we've found a new palindromic substring.
3.  **Summation**: The total count is the sum of palindromes found from all centers.

---

## Complexity Analysis

### Time Complexity

$O(n^2)$

- We iterate through $2n - 1$ centers.
- For each center, expansion takes at most $O(n)$ time.

### Space Complexity

$O(1)$

- We only use a few variables for counting and pointers. No extra data structures are required.

---

## Key Takeaways

- Similar to "Longest Palindromic Substring", the **center-expansion** technique is highly efficient for counting problems.
- Remember to handle both **odd** and **even** length centers.
- This approach avoids the $O(n^2)$ space requirement of the typical DP table approach.
