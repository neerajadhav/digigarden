---
title: 01 - String Reversal
tags:
  - Strings
  - Fundamentals
---

## Problem
Given a string, return its reverse. For example, if the input is `"hello"`, the output should be `"olleh"`.

## Intuition
The most efficient way to reverse a string (or any array-like structure) is using the **Two-Pointer** technique. We place one pointer at the start and another at the end of the character array. We swap the characters at these pointers and move the pointers towards each other until they meet.

## Algorithm Steps
1. Convert the string to a character array.
2. Initialize `left = 0` and `right = length - 1`.
3. While `left < right`:
    a. Swap `arr[left]` and `arr[right]`.
    b. Increment `left`.
    c. Decrement `right`.
4. Convert the character array back to a string and return it.

## Pseudocode
```text
procedure reverseString(S)
    arr := convert S to char array
    left := 0
    right := length(arr) - 1
    while left < right do
        swap(arr[left], arr[right])
        left := left + 1
        right := right - 1
    end while
    return convert arr to string
end procedure
```

## Java Code using JCF
Using a competitive programming structure. Note that strings in Java are immutable, so we typically use `StringBuilder` or `char[]`.

```java
import java.util.*;

public class Main {
    /**
     * Efficiently reverses a string using two pointers
     */
    public static String reverse(String s) {
        if (s == null) return null;
        char[] arr = s.toCharArray();
        int left = 0;
        int right = arr.length - 1;
        
        while (left < right) {
            char temp = arr[left];
            arr[left] = arr[right];
            arr[right] = temp;
            left++;
            right--;
        }
        return new String(arr);
    }

    /* Alternative using StringBuilder (built-in) */
    public static String reverseBuiltIn(String s) {
        return new StringBuilder(s).reverse().toString();
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        if (sc.hasNextInt()) {
            int t = sc.nextInt();
            sc.nextLine(); // Consume newline
            while (t-- > 0) {
                if (sc.hasNextLine()) {
                    String s = sc.nextLine();
                    String result = reverse(s);
                    System.out.println(result);
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
2
hello
algorithms
```

**Output:**
```text
olleh
smhtirogla
```

## Complexity
| Case | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **All Cases** | $O(n)$ | $O(n)$ (for the character array/result string) |

- **Type**: Iterative / Two Pointers.
- **In-place**: Yes (if modifying a character array); strings are immutable in Java.

## Example and Dry Run
**Input**: `s = "abcde"`

1. **Initial**: `left = 0 ('a')`, `right = 4 ('e')`.
2. **Swap**: `['e', 'b', 'c', 'd', 'a']`. `left = 1`, `right = 3`.
3. **Swap**: `['e', 'd', 'c', 'b', 'a']`. `left = 2`, `right = 2`.
4. **End**: `left >= right`. Loop terminates.

**Final Result**: `"edcba"`

## Variations
1. **Reverse in Batches**: Reverse every $k$ characters.
2. **Sentence Reversal**: Reverse the words in a sentence (e.g., `"the sky is blue"` $\rightarrow$ `"blue is sky the"`).
3. **Recursive Reversal**: Reversing using recursion (less efficient due to stack space).
