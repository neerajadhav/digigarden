---
title: 14 - Longest Repeating Character Replacement
tags:
  - String
  - Sliding-Window
  - Hash-Table
  - Medium
---

You are given a string `s` consisting of only uppercase English characters and an integer `k`. You can choose up to `k` characters of the string and replace them with any other uppercase English character.

After performing at most `k` replacements, return the length of the **longest substring** which contains only one distinct character.

---

## Examples

**Example 1**

Input: 
```
s = "XYYX", k = 2
```

Output: 
```
4
```

Explanation: Either replace the 'X's with 'Y's, or replace the 'Y's with 'X's.

---

**Example 2**

Input: 
```
s = "AAABABB", k = 1
```

Output: 
```
5
```

---

## Solution

### Java Code

```java
import java.util.HashMap;
import java.util.Collections;
import java.util.Map;

class Solution {
    public int characterReplacement(String s, int k) {
        Map<Character, Integer> countMap = new HashMap<>();
        int left = 0;
        int maxFreq = 0;
        int maxLength = 0;

        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);
            countMap.put(c, countMap.getOrDefault(c, 0) + 1);
            
            // Track the frequency of the most frequent character in the current window
            maxFreq = Math.max(maxFreq, countMap.get(c));

            // Window is valid if: (window size) - (max frequency) <= k
            // If invalid, move left pointer to shrink the window
            while ((right - left + 1) - maxFreq > k) {
                char leftChar = s.charAt(left);
                countMap.put(leftChar, countMap.get(leftChar) - 1);
                left++;
                // Note: We don't strictly need to update maxFreq here for O(n) optimality,
                // but we could recalculate if needed using Collections.max(countMap.values()).
            }

            maxLength = Math.max(maxLength, right - left + 1);
        }

        return maxLength;
    }
}
```

---

## Java Collections Framework Components Used

## 1. `HashMap`

### What it is
`HashMap` is a hash-table based implementation of the `Map` interface. It stores key-value pairs and allows for constant-time average performance for basic operations.

### How it is used
```java
Map<Character, Integer> countMap = new HashMap<>();
```
Used to store the frequency of each character within the current sliding window.

### Key Methods Used:
- `put(K key, V value)`: Updates the frequency of a character.
- `get(Object key)`: Retrieves the current count.
- `getOrDefault(Object key, V defaultValue)`: Simplifies the increment logic.

---

## 2. `Math.max()`
Used to track the `maxFreq` within the window and the global `maxLength`.

---

## Intuition

We use a **Sliding Window** to find the longest valid substring.

A window is "valid" if we can turn all characters in it into the same character using at most `k` replacements. 
- The best character to turn everything into is the one that appears most frequently in that window (`maxFreq`).
- Number of replacements needed = `(window size) - (maxFreq)`.
- If `replacements > k`, the window is invalid, so we shrink it from the left.

We keep expanding the `right` pointer and shrinking the `left` pointer as needed to maintain a valid window, recording the maximum size seen.

---

## Complexity Analysis

### Time Complexity

```
O(n)
```

Each character is visited at most twice (left and right pointers).

---

### Space Complexity

```
O(m)
```

Where `m` is the number of unique characters in the string (at most 26 for uppercase English).

---

## Key Takeaways

- **Sliding Window** is perfect for finding the longest contiguous range satisfying a condition.
- **HashMap** allows us to track character frequencies dynamically.
- The condition `(right - left + 1) - maxFreq <= k` is the core constraint of this problem.
