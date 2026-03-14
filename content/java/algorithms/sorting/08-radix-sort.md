---
title: 08 - Radix Sort
tags:
  - Sorting
  - Non-Comparison-Based
---

# Radix Sort

## Problem
Sort an array of non-negative integers by processing individual digits. Radix sort avoids comparison by grouping keys by the individual digits which share the same significant position and value.

## Intuition
Radix Sort is like sorting a stack of papers with 3-digit numbers. First, you sort them into 10 piles based on the last digit (0-9). Then you collect them in order and sort them again into 10 piles based on the middle digit. Finally, you sort them based on the first digit. By the end, the whole stack is perfectly sorted. It uses **Counting Sort** as a stable subroutine for each digit.

## Algorithm Steps
1. Find the maximum number in the array to know the number of digits.
2. For each significant digit (units, tens, hundreds, ...):
   - Perform a stable sort (like Counting Sort) based on that digit.
3. Continue until all digits have been processed.

## Pseudocode
```text
procedure radixSort(A)
    maxVal := findMax(A)
    for exp := 1; maxVal / exp > 0; exp := exp * 10 do
        countingSortByDigit(A, exp)
    end for
end procedure
```

## Java Code using JCF
Using a competitive programming structure (TCS NQT Style).

```java
import java.util.*;

public class Main {
    /**
     * Radix Sort Implementation
     */
    public static void radixSort(List<Integer> list) {
        if (list.isEmpty()) return;

        // Find the maximum number to know number of digits
        int max = Collections.max(list);

        // Do counting sort for every digit. exp is 10^i
        for (int exp = 1; max / exp > 0; exp *= 10) {
            countSort(list, exp);
        }
    }

    /**
     * Stable counting sort based on the digit represented by exp
     */
    private static void countSort(List<Integer> list, int exp) {
        int n = list.size();
        int[] output = new int[n];
        int[] count = new int[10];
        Arrays.fill(count, 0);

        // Store count of occurrences in count[]
        for (int i = 0; i < n; i++) {
            count[(list.get(i) / exp) % 10]++;
        }

        // Change count[i] so that count[i] now contains actual
        // position of this digit in output[]
        for (int i = 1; i < 10; i++) {
            count[i] += count[i - 1];
        }

        // Build the output array (backward for stability)
        for (int i = n - 1; i >= 0; i--) {
            int current = list.get(i);
            int digit = (current / exp) % 10;
            output[count[digit] - 1] = current;
            count[digit]--;
        }

        // Copy the output array to list
        for (int i = 0; i < n; i++) {
            list.set(i, output[i]);
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        if (sc.hasNextInt()) {
            int t = sc.nextInt();
            while (t-- > 0) {
                if (sc.hasNextInt()) {
                    int n = sc.nextInt();
                    List<Integer> list = new ArrayList<>();
                    for (int i = 0; i < n; i++) {
                        if (sc.hasNextInt()) {
                            list.add(sc.nextInt());
                        }
                    }
                    radixSort(list);
                    for (int i = 0; i < list.size(); i++) {
                        System.out.print(list.get(i) + (i == list.size() - 1 ? "" : " "));
                    }
                    System.out.println();
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
1
8
170 45 75 90 802 24 2 66
```

**Output:**
```text
2 24 45 66 75 80 90 170 802
```

## Complexity
| Case | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **All Cases** | $O(d \cdot (n + k))$ | $O(n + k)$ |

- **$n$**: Number of elements.
- **$k$**: Range of digits (usually 10 for decimal).
- **$d$**: Number of digits in the largest number.
- **Stable**: Yes.
- **In-place**: No.

## Example and Dry Run
**Input**: `[170, 45, 75, 90, 802, 24, 2, 66]`

1. **Sort by units digit**:
   - `[170, 90, 802, 2, 24, 45, 75, 66]` (Note: 170 and 90 end in 0, 802 and 2 end in 2, etc.)
2. **Sort by tens digit**:
   - `[802, 2, 24, 45, 66, 170, 75, 90]` (Sorted by 0, 2, 4, 6, 7, 7, 9)
3. **Sort by hundreds digit**:
   - `[2, 24, 45, 66, 75, 90, 170, 802]` (Sorted by 0s, then 1, then 8)

## Variations
1. **MSD Radix Sort**: Most Significant Digit first (recursive).
2. **LSD Radix Sort**: Least Significant Digit first (iterative, as shown above).
