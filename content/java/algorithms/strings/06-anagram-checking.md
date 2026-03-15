---
title: 06 - Anagram Checking
tags:
  - Strings
  - Fundamentals
  - Hashing
---

## Problem
Given two strings `s1` and `s2`, determine if they are anagrams of each other. Two strings are anagrams if they contain the same characters with the same frequencies, but possibly in a different order.

## Intuition
The core idea for checking anagrams is to compare the frequency of each character in both strings. If the lengths of the strings differ, they cannot be anagrams. Otherwise, we can use a frequency array (if the character set is limited, like lowercase English letters) or a Hash Map to count occurrences.

## Algorithm Steps
1. If lengths of `s1` and `s2` are not equal, return `false`.
2. Initialize a frequency array (e.g., of size 26 for English letters or 256 for extended ASCII).
3. Iterate through string `s1` and increment the count for each character in the frequency array.
4. Iterate through string `s2` and decrement the count for each character in the frequency array.
5. If all counts in the frequency array are zero, return `true`. Otherwise, return `false`.

## Pseudocode
```text
procedure areAnagrams(s1, s2)
    if length(s1) != length(s2) then
        return false
    end if
    
    count := array of size 26, initialized to 0
    for i := 0 to length(s1) - 1 do
        count[s1[i] - 'a'] := count[s1[i] - 'a'] + 1
        count[s2[i] - 'a'] := count[s2[i] - 'a'] - 1
    end for
    
    for i := 0 to 25 do
        if count[i] != 0 then
            return false
        end if
    end for
    
    return true
end procedure
```

## Java Code using JCF
Using a competitive programming structure.

```java
import java.util.*;

public class Main {
    /**
     * Checks if two strings are anagrams (lowercase English letters only)
     */
    public static boolean areAnagrams(String s1, String s2) {
        if (s1.length() != s2.length()) {
            return false;
        }

        int[] count = new int[26];
        for (int i = 0; i < s1.length(); i++) {
            count[s1.charAt(i) - 'a']++;
            count[s2.charAt(i) - 'a']--;
        }

        for (int c : count) {
            if (c != 0) {
                return false;
            }
        }
        return true;
    }

    /* Alternative using sorting (O(n log n)) */
    public static boolean areAnagramsSort(String s1, String s2) {
        if (s1.length() != s2.length()) return false;
        char[] c1 = s1.toCharArray();
        char[] c2 = s2.toCharArray();
        Arrays.sort(c1);
        Arrays.sort(c2);
        return Arrays.equals(c1, c2);
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        if (sc.hasNextInt()) {
            int t = sc.nextInt();
            while (t-- > 0) {
                if (sc.hasNext()) {
                    String s1 = sc.next();
                    if (sc.hasNext()) {
                        String s2 = sc.next();
                        boolean result = areAnagrams(s1.toLowerCase(), s2.toLowerCase());
                        System.out.println(result ? "YES" : "NO");
                    }
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
listen silent
anagram nagaram
hello world
```

**Output:**
```text
YES
YES
NO
```

## Complexity
| Case | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **Using Hash Array** | $O(n)$ | $O(1)$ (constant space for the alphabet size) |
| **Using Sorting** | $O(n \log n)$ | $O(n)$ or $O(1)$ depending on sort implementation |

- **Type**: Iterative / Frequency Counting.
- **Sorted Required**: No.
- **In-place**: Yes.

## Example and Dry Run
**Input**: `s1 = "listen"`, `s2 = "silent"`

1. **Lengths**: Both are 6. Proceed.
2. **Iteration**:
   - 'l': `count['l'-'a']++`, 's': `count['s'-'a']--`
   - 'i': `count['i'-'a']++`, 'i': `count['i'-'a']--`
   - 's': `count['s'-'a']++`, 'l': `count['l'-'a']--`
   - 't': `count['t'-'a']++`, 'e': `count['e'-'a']--`
   - 'e': `count['e'-'a']++`, 'n': `count['n'-'a']--`
   - 'n': `count['n'-'a']++`, 't': `count['t'-'a']--`
3. **Verify**: All entries in `count` are 0.

**Final Result**: YES

## Variations
1. **Group Anagrams**: Given an array of strings, group the anagrams together.
2. **Find All Anagrams in a String**: Find all starting indices of `pattern`'s anagrams in `text`.
3. **Valid Anagram (Unicode)**: Handle extended character sets using a Hash Map instead of a fixed-size array.
