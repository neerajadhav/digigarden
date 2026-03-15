---
title: 05 - Z-Algorithm
tags:
  - Strings
  - Pattern Matching
  - Optimization
---

## Problem
Given a text `T` and a pattern `P`, find all occurrences of `P` in `T`. The algorithm should find all matches in linear time ($O(n+m)$).

## Intuition
The Z-Algorithm relies on the **Z-array**, where each element $Z[i]$ represents the length of the longest common prefix (LCP) between the string $S$ and the suffix of $S$ starting at $i$.

By constructing a combined string $S = P + \$ + T$ (where `$` is a sentinel character not present in $P$ or $T$), any index $i$ in the Z-array where $Z[i] == \text{length}(P)$ indicates an occurrence of the pattern in the text.

The algorithm uses a "Z-box" $[L, R]$ to keep track of the rightmost segment of the string that matches a prefix of the string, allowing it to avoid redundant comparisons.

## Algorithm Steps
1. Concatenate $P$, a sentinel character `$`, and $T$ to form string $S$.
2. Initialize an array $Z$ of size `length(S)`.
3. Maintenance of $L$ and $R$:
    a. If $i > R$: Compare $S[i...]$ with $S[0...]$. Update $L, R$.
    b. If $i \leq R$:
        - Let $k = i - L$.
        - If $Z[k] < R - i + 1$: $Z[i] = Z[k]$.
        - Else: Compare $S[R+1...]$ with $S[R-i+1...]$ and update $L, R$.
4. Check all $Z[i]$ from $i = \text{length}(P) + 1$ to the end. If $Z[i] == \text{length}(P)$, pattern found.

## Pseudocode
```text
procedure computeZ(S)
    n := length(S)
    Z := new array of size n
    L, R := 0, 0
    for i := 1 to n - 1 do
        if i > R then
            L, R := i, i
            while R < n and S[R] == S[R - L] do
                R := R + 1
            end while
            Z[i] := R - L
            R := R - 1
        else
            k := i - L
            if Z[k] < R - i + 1 then
                Z[i] := Z[k]
            else
                L := i
                while R < n and S[R] == S[R - L] do
                    R := R + 1
                end while
                Z[i] := R - L
                R := R - 1
            end if
        end if
    end for
    return Z
end procedure
```

## Java Code using JCF
Using a competitive programming structure.

```java
import java.util.*;

public class Main {
    /**
     * Computes the Z-array for a given string
     */
    private static int[] computeZ(String s) {
        int n = s.length();
        int[] z = new int[n];
        int l = 0, r = 0;

        for (int i = 1; i < n; i++) {
            if (i > r) {
                l = r = i;
                while (r < n && s.charAt(r) == s.charAt(r - l)) {
                    r++;
                }
                z[i] = r - l;
                r--;
            } else {
                int k = i - l;
                if (z[k] < r - i + 1) {
                    z[i] = z[k];
                } else {
                    l = i;
                    while (r < n && s.charAt(r) == s.charAt(r - l)) {
                        r++;
                    }
                    z[i] = r - l;
                    r--;
                }
            }
        }
        return z;
    }

    /**
     * Z-Algorithm Pattern Searching
     */
    public static List<Integer> search(String text, String pattern) {
        List<Integer> result = new ArrayList<>();
        String concat = pattern + "$" + text;
        int l = pattern.length();
        int[] z = computeZ(concat);

        for (int i = 0; i < z.length; i++) {
            if (z[i] == l) {
                result.add(i - l - 1);
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
baabaa
aab
aaaaa
aa
```

**Output:**
```text
1
0 1 2 3
```

## Complexity
| Case | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **All Cases** | $O(n + m)$ | $O(n + m)$ |

- **Type**: Iterative / Z-Box.
- **Sorted Required**: No.

## Example and Dry Run
**Input**: `T = "baabaa"`, `P = "aab"`

1. **Concat**: `S = "aab$baabaa"`
2. **Z-array Computation**:
   - `S[0..2] = "aab"`
   - `Z` values for indices corresponding to `T`:
     - Index 4 ('b'): $0$
     - Index 5 ('a'): $0$
     - Index 6 ('a'): $Z[6] = 3$ (Prefix "aab" matches)
     - Index 7 ('b'): $0$
     - Index 8 ('a'): $0$
     - Index 9 ('a'): $0$

**Final Result**: `[1]` (matches `T` at index 1)

## Variations
1. **Finding String Period**: If $n$ is divisible by $(n - Z[i])$ and $Z[i] + i == n$.
2. **Longest Palindromic Prefix**: Use $S + \# + reverse(S)$.
3. **Pattern Matching with Wildcards**: Modified Z-algorithm to handle `?` or `*`.
