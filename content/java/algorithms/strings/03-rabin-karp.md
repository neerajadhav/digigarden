---
title: 03 - Rabin-Karp
tags:
  - Strings
  - Pattern Matching
  - Rolling Hash
---

## Problem
Given a text `T` and a pattern `P`, find all occurrences of `P` in `T`.

## Intuition
The Rabin-Karp algorithm uses **Rolling Hashing** to skip unnecessary comparisons. Instead of checking every character for every window (like a naive $O(n \cdot m)$ approach), it computes a hash of the pattern and compares it with the hash of the current window in the text. If hashes match, it performs a character-by-character check to handle potential collisions. A "rolling" hash allows us to compute the hash of the next window from the current one in $O(1)$ time.

## Algorithm Steps
1. Choose a prime number `q` (for modulo) and a base `d` (typically number of characters in alphabet).
2. Calculate the hash of the pattern `P` and the hash of the first window of text `T`.
3. For each window in the text:
    a. If `hash(P) == hash(window)`:
        - Verify characters one by one. If they match, record the index.
    b. If not the last window:
        - Calculate the hash of the next window using the rolling hash formula.
4. Return all found indices.

## Pseudocode
```text
procedure rabinKarp(T, P, d, q)
    n := length(T)
    m := length(P)
    h := d^(m-1) mod q
    pHash := 0
    tHash := 0
    
    for i := 0 to m-1 do
        pHash := (d * pHash + P[i]) mod q
        tHash := (d * tHash + T[i]) mod q
    end for
    
    for i := 0 to n - m do
        if pHash == tHash then
            if T[i...i+m-1] == P then
                print "Found at index " + i
            end if
        end if
        
        if i < n - m then
            tHash := (d * (tHash - T[i] * h) + T[i + m]) mod q
            if tHash < 0 then tHash := tHash + q
        end if
    end for
end procedure
```

## Java Code using JCF
Using a competitive programming structure.

```java
import java.util.*;

public class Main {
    // Base for the rolling hash (e.g., 256 for ASCII)
    private static final int D = 256;
    // A prime number for modulo calculations
    private static final int Q = 101; 

    /**
     * Rabin-Karp Pattern Searching Algorithm
     */
    public static List<Integer> search(String text, String pattern) {
        List<Integer> result = new ArrayList<>();
        int n = text.length();
        int m = pattern.length();
        if (m > n) return result;

        int pHash = 0; // hash value for pattern
        int tHash = 0; // hash value for text window
        int h = 1;

        // The value of h would be "pow(D, m-1) % Q"
        for (int i = 0; i < m - 1; i++) {
            h = (h * D) % Q;
        }

        // Calculate initial hash values
        for (int i = 0; i < m; i++) {
            pHash = (D * pHash + pattern.charAt(i)) % Q;
            tHash = (D * tHash + text.charAt(i)) % Q;
        }

        // Slide the pattern over text
        for (int i = 0; i <= n - m; i++) {
            // Check if hashes match
            if (pHash == tHash) {
                // Potential match, verify characters
                boolean match = true;
                for (int j = 0; j < m; j++) {
                    if (text.charAt(i + j) != pattern.charAt(j)) {
                        match = false;
                        break;
                    }
                }
                if (match) result.add(i);
            }

            // Calculate hash value for next window
            if (i < n - m) {
                tHash = (D * (tHash - text.charAt(i) * h) + text.charAt(i + m)) % Q;
                // Handle negative hash results
                if (tHash < 0) tHash = (tHash + Q);
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
                            for (int i = 0; i < result.size(); i++) {
                                System.out.print(result.get(i) + (i == result.size() - 1 ? "" : " "));
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
AABAACAADAABAABA
AABA
THIS IS A TEST TEXT
TEST
```

**Output:**
```text
0 9 12
10
```

## Complexity
| Case | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **Best/Average Case** | $O(n + m)$ | $O(1)$ |
| **Worst Case** | $O(n \cdot m)$ | $O(1)$ |

- **Type**: Iterative / Rolling Hash.
- **In-place**: Yes.
- **Sorted Required**: No.

## Example and Dry Run
**Input**: `T = "AABAACAADAABAABA"`, `P = "AABA"`

1. **Initial**: `hash(P) = X`, `hash(T[0..3]) = X`. Match found at index 0.
2. **Slide**: Move to index 1. `hash(T[1..4]) != X`.
3. **Slide**: Move to index 2. `hash(T[2..5]) != X`.
...
4. **Slide**: Move to index 9. `hash(T[9..12]) = X`. Match found at index 9.

**Final Result**: `[0, 9, 12]`

## Variations
1. **Multiple Pattern Matching**: Searching for multiple patterns simultaneously using Aho-Corasick or multiple rolling hashes.
2. **2D Pattern Matching**: Applying Rabin-Karp in two dimensions (e.g., finding a sub-matrix).
3. **Double Hashing**: Using two different prime numbers and bases to minimize the probability of collisions to negligible levels.
