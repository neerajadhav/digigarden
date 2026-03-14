---
title: 15 - Minimum Window Substring
tags:
  - String
  - Sliding-Window
  - Hash-Table
  - Hard
---

Given two strings `s` and `t`, return the **shortest substring** of `s` such that every character in `t`, (including duplicates), is present in the substring. If such a substring does not exist, return an empty string `""`.

You may assume that the correct output is always unique.

---

## Examples

**Example 1**

Input: 
```
s = "OUZODYXAZV", t = "XYZ"
```

Output: 
```
"YXAZ"
```

Explanation: "YXAZ" is the shortest substring that includes "X", "Y", and "Z" from string `t`.

---

**Example 2**

Input: 
```
s = "xyz", t = "xyz"
```

Output: 
```
"xyz"
```

---

**Example 3**

Input: 
```
s = "x", t = "xy"
```

Output: 
```
""
```

---

## Solution

### Java Code

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public String minWindow(String s, String t) {
        if (s.length() < t.length()) return "";

        // Map to store frequency of characters in t
        Map<Character, Integer> targetCount = new HashMap<>();
        for (char c : t.toCharArray()) {
            targetCount.put(c, targetCount.getOrDefault(c, 0) + 1);
        }

        // Map to store frequency of characters in current window
        Map<Character, Integer> windowCount = new HashMap<>();
        int left = 0, right = 0;
        int have = 0, need = targetCount.size();
        int[] res = {-1, -1}; // stores start and end of min window
        int minLen = Integer.MAX_VALUE;

        for (right = 0; right < s.length(); right++) {
            char c = s.charAt(right);
            windowCount.put(c, windowCount.getOrDefault(c, 0) + 1);

            // If current character is in t and its frequency matches, increment 'have'
            if (targetCount.containsKey(c) && 
                windowCount.get(c).equals(targetCount.get(c))) {
                have++;
            }

            // Shrink the window from the left as long as it is valid
            while (have == need) {
                // Update result if current window is smaller
                if ((right - left + 1) < minLen) {
                    minLen = right - left + 1;
                    res[0] = left;
                    res[1] = right;
                }

                // Remove character from left
                char leftChar = s.charAt(left);
                windowCount.put(leftChar, windowCount.get(leftChar) - 1);

                // If removing the character makes the window invalid, decrement 'have'
                if (targetCount.containsKey(leftChar) && 
                    windowCount.get(leftChar) < targetCount.get(leftChar)) {
                    have--;
                }
                left++;
            }
        }

        return minLen == Integer.MAX_VALUE ? "" : s.substring(res[0], res[1] + 1);
    }
}
```

---

## Java Collections Framework Components Used

## 1. `HashMap`

### What it is
A hash-table based implementation of the `Map` interface.

### How it is used
We use two `HashMap` instances:
1. `targetCount`: To store the required character frequencies from string `t`.
2. `windowCount`: To track the character frequencies in the currently active sliding window.

### Why used:
- Efficient frequency tracking.
- $O(1)$ average time complexity for `get`, `put`, and `containsKey`.
- **Duplicate Handling**: `HashMap` naturally handles cases where characters repeat in `t`.

---

## 2. `Map.getOrDefault()`
Used to handle character increments in a single line, avoiding verbose `if(contains)` checks.

---

## Intuition

This problem is solved using a **Sliding Window** with a "contract/expand" logic.

1. **Phase 1: Expand**: Move the `right` pointer until the current window contains all characters of `t` with at least their required frequencies. We track this using `have` (characters satisfied) and `need` (unique characters in `t`).
2. **Phase 2: Contract**: Once `have == need`, the window is valid. Now, attempt to shrink it from the `left` as much as possible while maintaining validity.
3. **Capture**: During contraction, record the smallest valid window found.

This approach ensures that we don't check redundant substrings, skipping straight to the "interesting" ones.

---

## Complexity Analysis

### Time Complexity

```
O(n + m)
```

Where `n` is the length of `s` and `m` is the length of `t`. We traverse `s` once with `right` and once with `left`.

---

### Space Complexity

```
O(k)
```

Where `k` is the number of unique characters in `s` and `t` (at most 52 for English letters).

---

## Key Takeaways

- **Sliding Window with Two Maps** is the standard approach for substring problems involving character sets.
- **Reference Comparison**: When using `equals()` with `HashMap.get()` values (which are `Integer` objects), be careful of object pooling; however, for frequencies, it's safer than `==`.
- This problem demonstrates how to solve a "Hard" complexity in linear time.
