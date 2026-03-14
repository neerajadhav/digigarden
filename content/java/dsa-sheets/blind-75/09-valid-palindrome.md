---
title: 09 - Valid Palindrome
tags:
  - String
  - Two-Pointers
  - Easy
---

Given a string `s`, return `true` if it is a palindrome, otherwise return `false`.

A **palindrome** is a string that reads the same forward and backward. It is also case-insensitive and ignores all non-alphanumeric characters.

**Note:** Alphanumeric characters consist of letters (A-Z, a-z) and numbers (0-9).

---

## Examples

**Example 1**

Input: 
```
s = "Was it a car or a cat I saw?"
```

Output: 
```
true
```

Explanation: After considering only alphanumerical characters we have "wasitacaroracatisaw", which is a palindrome.

---

**Example 2**

Input: 
```
s = "tab a cat"
```

Output: 
```
false
```

Explanation: "tabacat" is not a palindrome.

---

## Solution

### Java Code

```java
class Solution {
    public boolean isPalindrome(String s) {
        int left = 0;
        int right = s.length() - 1;

        while (left < right) {
            // Move left pointer until an alphanumeric character is found
            while (left < right && !Character.isLetterOrDigit(s.charAt(left))) {
                left++;
            }
            // Move right pointer until an alphanumeric character is found
            while (left < right && !Character.isLetterOrDigit(s.charAt(right))) {
                right--;
            }

            // Compare characters (case-insensitive)
            if (Character.toLowerCase(s.charAt(left)) != Character.toLowerCase(s.charAt(right))) {
                return false;
            }

            left++;
            right--;
        }

        return true;
    }
}
```

---

## Intuition

A string is a palindrome if it reads the same forward and backward. 

The most efficient way to check this without creating a new filtered string (which would use $O(n)$ space) is to use **two pointers**:
1. Place one pointer at the start and one at the end of the string.
2. Advance them toward the center.
3. Skip any non-alphanumeric characters along the way using `Character.isLetterOrDigit()`.
4. Compare the characters at the pointers (ignoring case). If they don't match, it's not a palindrome.

This approach ensures we only traverse the string once and don't use any extra memory proportional to the string length.

---

## Complexity Analysis

### Time Complexity

```
O(n)
```

Each character in the string `s` is visited at most twice (one by the inner `while` loops and once by the pointer increment/decrement).

---

### Space Complexity

```
O(1)
```

We only use two pointers (`left` and `right`) and no auxiliary data structures that grow with the input size.

---

## Key Takeaways

- **Two Pointers** is the gold standard for palindrome and string reversal problems.
- **In-place filtering** (skipping characters during traversal) saves space compared to `replaceAll()` or `StringBuilder`.
- `Character.isLetterOrDigit()` and `Character.toLowerCase()` are essential built-in tools for string manipulation in Java.
