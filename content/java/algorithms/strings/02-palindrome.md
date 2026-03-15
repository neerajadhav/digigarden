---
title: 02 - Palindrome
tags:
  - Strings
  - Two Pointers
  - Fundamentals
---

## Problem
Given a string, determine if it is a palindrome. A string is a palindrome if it reads the same forward and backward, ignoring case and non-alphanumeric characters.

## Intuition
To check if a string is a palindrome, we can utilize the **Two-Pointer** technique. By placing one pointer at the start and another at the end, we can compare the characters. If they match, we move both pointers towards the center. If we encounter a mismatch, the string is not a palindrome.

## Algorithm Steps
1. Initialize `left = 0` and `right = length - 1`.
2. While `left < right`:
    a. Skip non-alphanumeric characters for `left` and `right` (if required by problem).
    b. Compare characters at `left` and `right` (case-insensitive).
    c. If they don't match, return `false`.
    d. Increment `left` and decrement `right`.
3. If the loop completes, return `true`.

## Pseudocode
```text
procedure isPalindrome(S)
    left := 0
    right := length(S) - 1
    while left < right do
        while left < right and not isAlphanumeric(S[left]) do
            left := left + 1
        end while
        while left < right and not isAlphanumeric(S[right]) do
            right := right - 1
        end while
        
        if toLower(S[left]) != toLower(S[right]) then
            return false
        end if
        
        left := left + 1
        right := right - 1
    end while
    return true
end procedure
```

## Java Code using JCF
Using a competitive programming structure.

```java
import java.util.*;

public class Main {
    /**
     * Checks if a string is a palindrome (case-insensitive, alphanumeric only)
     */
    public static boolean isPalindrome(String s) {
        if (s == null) return false;
        
        int left = 0;
        int right = s.length() - 1;
        
        while (left < right) {
            char l = s.charAt(left);
            char r = s.charAt(right);
            
            if (!Character.isLetterOrDigit(l)) {
                left++;
            } else if (!Character.isLetterOrDigit(r)) {
                right--;
            } else {
                if (Character.toLowerCase(l) != Character.toLowerCase(r)) {
                    return false;
                }
                left++;
                right--;
            }
        }
        return true;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        if (sc.hasNextInt()) {
            int t = sc.nextInt();
            sc.nextLine(); // Consume newline
            while (t-- > 0) {
                if (sc.hasNextLine()) {
                    String s = sc.nextLine();
                    boolean result = isPalindrome(s);
                    System.out.println(result ? "YES" : "NO");
                }
            }
        }
        sc.close();
    }
}
```

### Sample Input and Output

**Input:**
```text
3
A man, a plan, a canal: Panama
race a car
aba
```

**Output:**
```text
YES
NO
YES
```

## Complexity
| Case | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **All Cases** | $O(n)$ | $O(1)$ |

- **Type**: Iterative / Two Pointers.
- **Sorted Required**: No.
- **In-place**: Yes.

## Example and Dry Run
**Input**: `s = "racecar"`

1. **left = 0 ('r'), right = 6 ('r')**: Match. `left = 1, right = 5`.
2. **left = 1 ('a'), right = 5 ('a')**: Match. `left = 2, right = 4`.
3. **left = 2 ('c'), right = 4 ('c')**: Match. `left = 3, right = 3`.
4. **End**: `left >= right`. Loop terminates.

**Final Result**: YES

## Variations
1. **Valid Palindrome II**: Determine if a string can be a palindrome after deleting at most one character.
2. **Longest Palindromic Substring**: Find the longest contiguous segment that is a palindrome ($O(n^2)$ or $O(n)$ with Manacher's).
3. **Palindrome Partitioning**: Split a string into all possible palindromic substrings.
