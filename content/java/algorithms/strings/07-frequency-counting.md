---
title: 07 - Frequency Counting
tags:
  - Strings
  - Fundamentals
  - Hashing
---

## Problem
Given a string, count the number of occurrences of each character. This is a fundamental operation used in many algorithms like string compression, anagram checking, and Huffman coding.

## Intuition
To count character frequencies efficiently, we need a way to store and update the count for each unique character encountered. 
1. If the character set is small and fixed (e.g., lowercase English letters), an **integer array** of size 26 is the most efficient choice where the index represents the character.
2. For a general character set (like Unicode), a **Hash Table** (`HashMap` in Java) is used to map characters to their respective counts.

## Algorithm Steps
1. **Using an Array**:
    a. Initialize an array of size 26 (initialized to 0).
    b. Iterate through each character `c` in the string.
    c. Increment the value at index `c - 'a'`.
2. **Using a Hash Map**:
    a. Initialize an empty Hash Map.
    b. Iterate through each character `c` in the string.
    c. If `c` exists in the map, increment its value; otherwise, add it with a count of 1.

## Pseudocode
```text
procedure countFrequency(S)
    freq := array of size 26, initialized to 0
    for each char c in S do
        freq[c - 'a'] := freq[c - 'a'] + 1
    end for
    return freq
end procedure
```

## Java Code using JCF
Using a competitive programming structure.

```java
import java.util.*;

public class Main {
    /**
     * Iterative frequency count using an array (optimized for lowercase English)
     */
    public static void countArray(String s) {
        int[] freq = new int[26];
        for (char c : s.toCharArray()) {
            if (c >= 'a' && c <= 'z') {
                freq[c - 'a']++;
            }
        }
        
        // Print non-zero frequencies
        for (int i = 0; i < 26; i++) {
            if (freq[i] > 0) {
                System.out.println((char)(i + 'a') + ": " + freq[i]);
            }
        }
    }

    /**
     * Frequency count using HashMap (handles any character)
     */
    public static void countMap(String s) {
        Map<Character, Integer> map = new LinkedHashMap<>(); // Maintains insertion order
        for (char c : s.toCharArray()) {
            map.put(c, map.getOrDefault(c, 0) + 1);
        }
        
        for (Map.Entry<Character, Integer> entry : map.entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue());
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        if (sc.hasNextInt()) {
            int t = sc.nextInt();
            while (t-- > 0) {
                if (sc.hasNext()) {
                    String s = sc.next();
                    // Choose approach based on problem requirements
                    countArray(s.toLowerCase());
                    System.out.println("---");
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
success
```

**Output:**
```text
e: 1
h: 1
l: 2
o: 1
---
c: 2
e: 1
s: 3
u: 1
---
```

## Complexity
| Method | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **Array-based** | $O(n)$ | $O(1)$ (constant size 26) |
| **Hash Map-based** | $O(n)$ | $O(k)$ (where $k$ is unique characters) |

- **Type**: Iterative / Hashing.
- **In-place**: No (requires auxiliary storage).
- **Sorted Required**: No.

## Example and Dry Run
**Input**: `s = "abbc"`

1. **Initial**: `freq = [0, 0, 0, ...]` (26 slots)
2. **char = 'a'**: `freq['a'-'a']` (index 0) becomes 1.
3. **char = 'b'**: `freq['b'-'a']` (index 1) becomes 1.
4. **char = 'b'**: `freq['b'-'a']` (index 1) becomes 2.
5. **char = 'c'**: `freq['c'-'a']` (index 2) becomes 1.

**Result**: `a: 1, b: 2, c: 1`

## Variations
1. **First Unique Character**: Find the character that appears exactly once and has the smallest index.
2. **K-Frequent Elements**: Find characters that appear at least $k$ times.
3. **Sort by Frequency**: Return characters in descending order of their counts.
4. **Isomorphic Strings**: Use two frequency maps (or arrays) to check mapping.
