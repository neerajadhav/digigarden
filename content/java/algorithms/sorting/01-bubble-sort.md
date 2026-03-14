---
title: 01 - Bubble Sort
tags:
  - Sorting
---

# Bubble Sort

## Problem
Given an unsorted array or list of elements, sort them in ascending (or descending) order by repeatedly swapping adjacent elements if they are in the wrong order.

## Intuition
The algorithm gets its name because smaller (or larger) elements "bubble" up to their correct positions at the end of each pass. Imagine a row of bubbles in water; the largest bubbles rise to the top fastest. In Bubble Sort, in each pass, the largest unsorted element "bubbles up" to its correct position at the end of the array.

## Algorithm Steps
1. Start with the first element (index 0).
2. Compare the current element with the next element.
3. If the current element is greater than the next element, swap them.
4. Move to the next pair of elements and repeat steps 2-3 until the end of the unsorted part of the array.
5. After one full pass, the largest element is at the last position.
6. Repeat the process for the remaining $n-1$ elements, then $n-2$, and so on, until the entire array is sorted.

## Pseudocode
```text
procedure bubbleSort(A : list of sortable items)
    n := length(A)
    repeat
        swapped := false
        for i := 1 to n-1 inclusive do
            if A[i-1] > A[i] then
                swap(A[i-1], A[i])
                swapped := true
            end if
        end for
        n := n - 1
    until not swapped
end procedure
```

## Java Code using JCF
Using `ArrayList` and `Collections.swap` with a test-case-driven approach (Common in TCS NQT/Competitive Programming).

```java
import java.util.*;

public class Main {
    /**
     * Optimized Bubble Sort using List and Collections.swap
     */
    public static void bubbleSort(List<Integer> list) {
        int n = list.size();
        boolean swapped;
        for (int i = 0; i < n - 1; i++) {
            swapped = false;
            for (int j = 0; j < n - i - 1; j++) {
                if (list.get(j) > list.get(j + 1)) {
                    Collections.swap(list, j, j + 1);
                    swapped = true;
                }
            }
            if (!swapped) break;
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        // Input: Number of test cases
        if (sc.hasNextInt()) {
            int t = sc.nextInt();
            
            while (t-- > 0) {
                // Input: Size of array
                if (sc.hasNextInt()) {
                    int n = sc.nextInt();
                    List<Integer> list = new ArrayList<>();
                    
                    // Input: Array elements
                    for (int i = 0; i < n; i++) {
                        if (sc.hasNextInt()) {
                            list.add(sc.nextInt());
                        }
                    }
                    
                    bubbleSort(list);
                    
                    // Output: Sorted array
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
2
5
5 1 4 2 8
7
64 34 25 12 22 11 90
```

**Output:**
```text
1 2 4 5 8
11 12 22 25 34 64 90
```

## Complexity
| Case | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **Best Case** | $O(n)$ (when already sorted) | $O(1)$ |
| **Average Case** | $O(n^2)$ | $O(1)$ |
| **Worst Case** | $O(n^2)$ | $O(1)$ |

- **Stable**: Yes (does not change the relative order of equal elements).
- **In-place**: Yes (requires only a constant amount of extra space).

## Example and Dry Run
**Input**: `[5, 1, 4, 2, 8]`

**Pass 1**:
1. `(5, 1, 4, 2, 8)` $\rightarrow$ `(1, 5, 4, 2, 8)`: $5 > 1$, swap.
2. `(1, 5, 4, 2, 8)` $\rightarrow$ `(1, 4, 5, 2, 8)`: $5 > 4$, swap.
3. `(1, 4, 5, 2, 8)` $\rightarrow$ `(1, 4, 2, 5, 8)`: $5 > 2$, swap.
4. `(1, 4, 2, 5, 8)` $\rightarrow$ `(1, 4, 2, 5, 8)`: $5 < 8$, no swap.
*Largest element 8 is now at the end.*

**Pass 2**:
1. `(1, 4, 2, 5, 8)` $\rightarrow$ `(1, 4, 2, 5, 8)`: $1 < 4$, no swap.
2. `(1, 4, 2, 5, 8)` $\rightarrow$ `(1, 2, 4, 5, 8)`: $4 > 2$, swap.
3. `(1, 2, 4, 5, 8)` $\rightarrow$ `(1, 2, 4, 5, 8)`: $4 < 5$, no swap.
*Next largest element 5 is now at the correctly sorted position.*

**Pass 3**:
1. `(1, 2, 4, 5, 8)` $\rightarrow$ `(1, 2, 4, 5, 8)`: $1 < 2$, no swap.
2. `(1, 2, 4, 5, 8)` $\rightarrow$ `(1, 2, 4, 5, 8)`: $2 < 4$, no swap.
*No swaps in Pass 3 means array is sorted!*

## Variations
1. **Optimized Bubble Sort**: Includes a flag (`swapped`) to stop early if the array becomes sorted before all passes are complete.
2. **Cocktail Shaker Sort**: A bidirectional bubble sort that bubbles in both directions (left-to-right and right-to-left) to handle "turtles" (small elements at the end) more efficiently.
3. **Odd-Even Sort**: Parallel version of bubble sort where comparisons happen in two phases (odd-even and even-odd).
