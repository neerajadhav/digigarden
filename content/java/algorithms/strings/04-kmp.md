---
title: 04 - KMP Algorithm
tags:
  - Strings
  - Pattern Matching
  - Optimization
---

## Problem
Given a text `T` and a pattern `P`, find all occurrences of `P` in `T`. The algorithm should minimize redundant comparisons.

## Intuition
The Knuth-Morris-Pratt (KMP) algorithm observes that whenever we detect a mismatch, the pattern itself contains enough information to determine where the next match could begin, thus bypassing re-evaluating characters that are already known to match.

We precompute a **Longest Proper Prefix which is also a Suffix (LPS)** array for the pattern. `LPS[i]` stores the length of the longest proper prefix of `P[0...i]` that is also a suffix of `P[0...i]`.

## Algorithm Steps
1. **Precomputation**: Build the `LPS` array for pattern `P`.
2. **Searching**:
    a. Use two pointers: `i` for text `T` and `j` for pattern `P`.
    b. If characters match (`T[i] == P[j]`), increment both `i` and `j`.
    c. If a full match is found (`j == m`), record the index and reset `j = LPS[j-1]`.
    d. If a mismatch occurs after some matches:
        - If `j > 0`, set `j = LPS[j-1]` (do not increment `i`).
        - If `j == 0`, increment `i`.

## Pseudocode
```text
procedure computeLPS(P)
    m := length(P)
    LPS := new array of size m
    len := 0
    i := 1
    while i < m do
        if P[i] == P[len] then
            len := len + 1
            LPS[i] := len
            i := i + 1
        else
            if len != 0 then
                len := LPS[len-1]
            else
                LPS[i] := 0
                i := i + 1
            end if
        end if
    end while
    return LPS
end procedure

procedure KMPSearch(T, P)
    n := length(T)
    m := length(P)
    LPS := computeLPS(P)
    i := 0
    j := 0
    while i < n do
        if T[i] == P[j] then
            i := i + 1
            j := j + 1
        end if
        if j == m then
            print "Found at index " + (i - j)
            j := LPS[j-1]
        else if i < n and T[i] != P[j] then
            if j != 0 then
                j := LPS[j-1]
            else
                i := i + 1
            end if
        end if
    end while
end procedure
```

## Java Code using JCF
Using a competitive programming structure.

```java
import java.util.*;

public class Main {
    /**
     * Builds the Longest Proper Prefix which is also Suffix array
     */
    private static int[] computeLPS(String pattern) {
        int m = pattern.length();
        int[] lps = new int[m];
        int len = 0;
        int i = 1;

        while (i < m) {
            if (pattern.charAt(i) == pattern.charAt(len)) {
                len++;
                lps[i] = len;
                i++;
            } else {
                if (len != 0) {
                    len = lps[len - 1];
                } else {
                    lps[i] = 0;
                    i++;
                }
            }
        }
        return lps;
    }

    /**
     * KMP Pattern Searching
     */
    public static List<Integer> search(String text, String pattern) {
        List<Integer> result = new ArrayList<>();
        int n = text.length();
        int m = pattern.length();
        if (m == 0) return result;

        int[] lps = computeLPS(pattern);
        int i = 0; // index for text
        int j = 0; // index for pattern

        while (i < n) {
            if (text.charAt(i) == pattern.charAt(j)) {
                i++;
                j++;
            }
            if (j == m) {
                result.add(i - j);
                j = lps[j - 1];
            } else if (i < n && text.charAt(i) != pattern.charAt(j)) {
                if (j != 0) {
                    j = lps[j - 1];
                } else {
                    i++;
                }
            }
        }
        return result;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        if (sc.hasNextInt()) {
            int t = sc.nextInt();
            sc.nextLine(); // Consume newline
            while (t-- > 0) {
                if (sc.hasNextLine()) {
                    String text = sc.nextLine();
                    if (sc.hasNextLine()) {
                        String pattern = sc.nextLine();
                        List<Integer> result = search(text, pattern);
                        
                        if (result.isEmpty()) {
                            System.out.println("-1");
                        } else {
                            for (int k = 0; k < result.size(); k++) {
                                System.out.print(result.get(k) + (k == result.size() - 1 ? "" : " "));
                            }
                            System.out.println();
                        }
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
2
ABABDABACDABABCABAB
ABABCABAB
AAAAA
AA
```

**Output:**
```text
10
0 1 2 3
```

## Complexity
| Step | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **Precomputation** | $O(m)$ | $O(m)$ |
| **Searching** | $O(n)$ | $O(m)$ |

- **Type**: Iterative / Finite Automata.
- **In-place**: Searching is in-place ($O(m)$ space for LPS).
- **Sorted Required**: No.

## Example and Dry Run
**Input**: `T = "ABABDABACDABABCABAB"`, `P = "ABABCABAB"`

1. **Precompute LPS**:
   - `P = A B A B C A B A B`
   - `LPS = [0, 0, 1, 2, 0, 1, 2, 3, 4]`

2. **Search**:
   - `i` runs from 0 to 18.
   - When a mismatch occurs at index `j`, `j` jumps to `LPS[j-1]`.
   - Pattern found at index 10.

**Final Result**: `[10]`

## Variations
1. **String Periodicity**: If $n$ is divisible by $(n - LPS[n-1])$, then the string has a period of length $(n - LPS[n-1])$.
2. **Counting Pattern Occurrences**: Standard use of the algorithm.
3. **Minimum Characters to make Palindrome**: Using KMP with $S + \# + reverse(S)$.
