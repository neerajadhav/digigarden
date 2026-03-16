---
title: 49 - Longest Palindromic Substring
tags:
  - Dynamic Programming
  - String
  - Two Pointers
  - Medium
---

Given a string `s`, return the longest substring of `s` that is a palindrome.

A palindrome is a string that reads the same forward and backward.

If there are multiple palindromic substrings that have the same length, return any one of them.

---

## Examples

**Example 1**

Input: `s = "ababd"`  
Output: `"bab"`  
Explanation: Both "aba" and "bab" are valid answers.

**Example 2**

Input: `s = "abbc"`  
Output: `"bb"`

---

## Solution

### Java Code

```java
class Solution {
    public String longestPalindrome(String s) {
        if (s == null || s.length() == 0) {
            return "";
        }

        int start = 0;
        int end = 0;

        for (int i = 0; i < s.length(); i++) {
            // Case 1: Palindrome with odd length (centered at i)
            int len1 = expandAroundCenter(s, i, i);
            // Case 2: Palindrome with even length (centered between i and i+1)
            int len2 = expandAroundCenter(s, i, i + 1);

            int len = Math.max(len1, len2);

            if (len > end - start) {
                // Adjust start and end indices
                // If len is odd, start = i - (len-1)/2, end = i + (len-1)/2
                // If len is even, start = i - (len-2)/2, end = i + 1 + (len-2)/2
                // This formula works for both:
                start = i - (len - 1) / 2;
                end = i + len / 2;
            }
        }

        return s.substring(start, end + 1);
    }

    private int expandAroundCenter(String s, int left, int right) {
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            left--;
            right++;
        }
        // The length is (right - 1) - (left + 1) + 1 = right - left - 1
        return right - left - 1;
    }
}
```

---

## Intuition

A palindrome mirrors itself around its center. There are $2n - 1$ such centers for a string of length $n$ (either on a character or between two characters).

### Expand Around Center Approach
Instead of building a DP table, we can simply expand from each possible center and track the maximum length found.

1.  **Iterate**: Loop through each character $i$.
2.  **Expand**:
    - Try to find an **odd-length** palindrome centered at $i$.
    - Try to find an **even-length** palindrome centered between $i$ and $i+1$.
3.  **Update**: If a longer palindrome is found, update the `start` and `end` pointers.
4.  **Result**: Return the substring using the final `start` and `end` indices.

---

## Complexity Analysis

### Time Complexity

$O(n^2)$

- We iterate through the string $n$ times.
- For each index, we expand in both directions, which takes at most $O(n)$ time.

### Space Complexity

$O(1)$

- Unlike the standard DP table approach ($O(n^2)$ space), this method only uses constant extra space.

---

## Key Takeaways

- "Expand Around Center" is often the most intuitive and space-efficient way to solve palindromic substring problems.
- Don't forget that palindromes can have both **odd** and **even** lengths.
- Slicing strings in Java (`substring`) is $O(n)$, but the pointers themselves are $O(1)$.
