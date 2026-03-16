---
title: 51 - Decode Ways
tags:
  - Dynamic Programming
  - String
  - Medium
---

A string consisting of uppercase english characters can be encoded to a number using the following mapping:

'A' -> "1"
'B' -> "2"
...
'Z' -> "26"

To decode a message, digits must be grouped and then mapped back into letters using the reverse of the mapping above. There may be multiple ways to decode a message.

Given a string `s` containing only digits, return the number of ways to decode it.

---

## Examples

**Example 1**

Input: `s = "12"`  
Output: `2`  
Explanation: "12" could be decoded as "AB" (1 2) or "L" (12).

**Example 2**

Input: `s = "01"`  
Output: `0`  
Explanation: "01" cannot be decoded because "01" cannot be mapped into a letter.

---

## Solution

### Java Code

```java
class Solution {
    public int numDecodings(String s) {
        if (s == null || s.length() == 0 || s.charAt(0) == '0') {
            return 0;
        }

        // prev2 is dp[i-2], prev1 is dp[i-1]
        int prev2 = 1;
        int prev1 = 1;

        for (int i = 1; i < s.length(); i++) {
            int current = 0;
            
            // Ways using only the current character (single digit)
            if (s.charAt(i) != '0') {
                current += prev1;
            }
            
            // Ways using the current and previous character (two digits)
            int twoDigit = Integer.parseInt(s.substring(i - 1, i + 1));
            if (twoDigit >= 10 && twoDigit <= 26) {
                current += prev2;
            }
            
            // If at any point it's impossible to decode, we can return 0
            if (current == 0) return 0;
            
            prev2 = prev1;
            prev1 = current;
        }

        return prev1;
    }
}
```

---

## Intuition

This problem can be broken down into subproblems. If we know the number of ways to decode the prefix $s[0...i-1]$ and $s[0...i-2]$, we can determine the ways for $s[0...i]$.

### Dynamic Programming
A digit $s[i]$ can be decoded as:
1.  **A single digit**: Valid if $s[i] \ne '0'$.
2.  **A part of a two-digit number**: Valid if the number formado by $s[i-1]$ and $s[i]$ is between $10$ and $26$.

The recurrence is:
$$dp[i] = (\text{if valid single}) \cdot dp[i-1] + (\text{if valid pair}) \cdot dp[i-2]$$

### Optimization
Since we only ever look back at the most recent two results, we can replace the $O(n)$ space array with just two integer variables.

---

## Complexity Analysis

### Time Complexity

$O(n)$

- We iterate through the string of length $n$ exactly once.

### Space Complexity

$O(1)$

- We only use constant space for the two state variables.

---

## Key Takeaways

- Handling **leading zeros** and the digit '0' in general is the most important edge case.
- Multiple sub-decisions at each step (one digit vs two) is a hallmark of **linear DP**.
- Always look for space optimization in Fibonacci-like recurrence problems.
