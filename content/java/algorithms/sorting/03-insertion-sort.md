---
title: 03 - Insertion Sort
tags:
  - Sorting
---

## Problem
Sort an array by taking one element at a time and inserting it into its correct position among the already sorted elements.

## Intuition
Think of sorting a deck of cards in your hands. You start with one card (already sorted). Then you take the next card and find its correct position among the cards you are holding by shifting larger cards to the right.

## Algorithm Steps
1. Assume the first element is already sorted.
2. Pick the next element (the "key").
3. Compare the key with elements in the sorted subarray (to its left).
4. Shift elements that are greater than the key to one position ahead of their current position.
5. Insert the key into its correct slot.
6. Repeat until the entire array is sorted.

## Pseudocode
```text
procedure insertionSort(A : list of sortable items)
    n := length(A)
    for i := 1 to n-1 inclusive do
        key := A[i]
        j := i - 1
        while j >= 0 and A[j] > key do
            A[j + 1] := A[j]
            j := j - 1
        end while
        A[j + 1] := key
    end for
end procedure
```

## Java Code using JCF
Using `ArrayList` with a competitive programming structure (TCS NQT Style). Note: While JCF `List` is used, the logic involves manual shifting for standard Insertion Sort behavior.

```java
import java.util.*;

public class Main {
    /**
     * Insertion Sort on a List
     */
    public static void insertionSort(List<Integer> list) {
        int n = list.size();
        for (int i = 1; i < n; i++) {
            int key = list.get(i);
            int j = i - 1;

            // Move elements of list.get(0..i-1) that are greater than key
            // to one position ahead of their current position
            while (j >= 0 && list.get(j) > key) {
                list.set(j + 1, list.get(j));
                j = j - 1;
            }
            list.set(j + 1, key);
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
                    
                    insertionSort(list);
                    
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
12 11 13 5 6
4
4 3 2 1
```

**Output:**
```text
5 6 11 12 13
1 2 3 4
```

## Complexity
| Case | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **Best Case** | $O(n)$ (when already sorted) | $O(1)$ |
| **Average Case** | $O(n^2)$ | $O(1)$ |
| **Worst Case** | $O(n^2)$ (sorted in reverse) | $O(1)$ |

- **Stable**: Yes.
- **In-place**: Yes.
- **Online**: Yes (can sort a list as it receives it).

## Example and Dry Run
**Input**: `[12, 11, 13, 5, 6]`

**Pass 1** ($i=1$):
- Key = 11
- 12 > 11, shift 12.
- Array: `[12, 12, 13, 5, 6]`
- Insert key at index 0.
- Result: `[11, 12, 13, 5, 6]`

**Pass 2** ($i=2$):
- Key = 13
- 12 < 13, no shift.
- Result: `[11, 12, 13, 5, 6]`

**Pass 3** ($i=3$):
- Key = 5
- 13 > 5, 12 > 5, 11 > 5. Shift all.
- Array: `[11, 11, 12, 13, 6]` (intermediate)
- Insert key at index 0.
- Result: `[5, 11, 12, 13, 6]`

**Pass 4** ($i=4$):
- Key = 6
- 13 > 6, 12 > 6, 11 > 6. Shift those.
- Array: `[5, 11, 11, 12, 13]` (intermediate)
- Insert key at index 1.
- Result: `[5, 6, 11, 12, 13]`

## Variations
1. **Binary Insertion Sort**: Uses binary search to find the correct location to insert the selected item at each iteration, reducing comparisons to $O(n \log n)$.
2. **Shell Sort**: An optimization of insertion sort that allows the exchange of items that are far apart.
