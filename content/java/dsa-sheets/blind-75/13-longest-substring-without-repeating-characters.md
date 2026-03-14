---
title: 13 - Longest Substring Without Repeating Chars
tags:
  - String
  - Sliding-Window
  - HashSet
  - Medium
---

Given a string `s`, find the length of the **longest substring** without duplicate characters.

A **substring** is a contiguous sequence of characters within a string.

---

## Examples

**Example 1**

Input: 
```
s = "zxyzxyz"
```

Output: 
```
3
```

Explanation: The string "xyz" is the longest without duplicate characters.

---

**Example 2**

Input: 
```
s = "xxxx"
```

Output: 
```
1
```

---

## Solution

### Java Code

```java
import java.util.HashSet;
import java.util.Set;

class Solution {
    public int lengthOfLongestSubstring(String s) {
        Set<Character> charSet = new HashSet<>();
        int left = 0;
        int maxLength = 0;

        for (int right = 0; right < s.length(); right++) {
            // If character already exists, shrink the window from the left
            while (charSet.contains(s.charAt(right))) {
                charSet.remove(s.charAt(left));
                left++;
            }
            
            // Add current character and update max length
            charSet.add(s.charAt(right));
            maxLength = Math.max(maxLength, right - left + 1);
        }

        return maxLength;
    }
}
```

---

## Java Collections Framework Components Used

## 1. `HashSet`

### What it is
`HashSet` is a collection that stores unique elements only. It is part of the `java.util` package and uses a hash table for storage.

### How it is used
```java
Set<Character> charSet = new HashSet<>();
```
Used to keep track of characters present in the current sliding window.

### Key Methods Used:
- `contains(Object o)`: Checks if a character is already in our window ($O(1)$ average).
- `add(E e)`: Adds a character to the window when expanding ($O(1)$ average).
- `remove(Object o)`: Removes a character from the window when shrinking from the left ($O(1)$ average).

---

## 2. `Math.max()`
Used to keep track of the largest window size encountered during the traversal.

---

## Intuition

We use a **Sliding Window** approach with two pointers (`left` and `right`).

1. **Expand**: Move the `right` pointer to include the next character.
2. **Check**: If the character at `right` is already in our `HashSet`, it means we have a duplicate.
3. **Shrink**: Increment the `left` pointer and remove characters from the `HashSet` until the duplicate is gone.
4. **Record**: At each step, update the `maxLength` with the size of the current unique substring window (`right - left + 1`).

This ensures we check all possible unique substrings in a single pass.

---

## Complexity Analysis

### Time Complexity

```
O(n)
```

Each character is visited at most twice: once by the `right` pointer and once by the `left` pointer.

---

### Space Complexity

```
O(min(m, n))
```

The size of the `HashSet` is bounded by the size of the string (`n`) and the size of the character set (`m`, e.g., 256 for extended ASCII).

---

## Key Takeaways

- **Sliding Window + HashSet** is the go-to pattern for finding unique contiguous sequences.
- Using a `Set` allows for $O(1)$ lookup, which is critical for maintaining linear time complexity.
- The window size is simply `right - left + 1`.
