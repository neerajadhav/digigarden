---
title: 02 - Selection Sort
tags:
  - Sorting
---

# Selection Sort

## Problem
Sort an array by repeatedly finding the minimum element (considering ascending order) from the unsorted part and putting it at the beginning.

## Intuition
Selection Sort maintains two subarrays in a given array:
1. The subarray which is already sorted.
2. Remaining subarray which is unsorted.

In every iteration of selection sort, the minimum element from the unsorted subarray is picked and moved to the sorted subarray.

## Algorithm Steps
1. Set `MIN_IDX` to location 0.
2. Search the minimum element in the list.
3. Swap with value at location `MIN_IDX`.
4. Increment `MIN_IDX` to point to next element.
5. Repeat until the list is sorted.

## Pseudocode
```text
procedure selectionSort(A : list of sortable items)
    n := length(A)
    for i := 0 to n-2 inclusive do
        min_idx := i
        for j := i+1 to n-1 inclusive do
            if A[j] < A[min_idx] then
                min_idx := j
            end if
        end for
        if min_idx != i then
            swap(A[i], A[min_idx])
        end if
    end for
end procedure
```

## Java Code using JCF
Using `ArrayList` and `Collections.swap` with a test-case-driven approach (TCS NQT Style).

```java
import java.util.*;

public class Main {
    /**
     * Selection Sort using List and Collections.swap
     */
    public static void selectionSort(List<Integer> list) {
        int n = list.size();
        for (int i = 0; i < n - 1; i++) {
            // Find the minimum element in unsorted array
            int min_idx = i;
            for (int j = i + 1; j < n; j++) {
                if (list.get(j) < list.get(min_idx)) {
                    min_idx = j;
                }
            }
            // Swap the found minimum element with the first element
            if (min_idx != i) {
                Collections.swap(list, min_idx, i);
            }
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
                    
                    selectionSort(list);
                    
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
64 25 12 22 11
6
5 4 3 2 1 10
```

**Output:**
```text
11 12 22 25 64
1 2 3 4 5 10
```

## Complexity
| Case | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **Best Case** | $O(n^2)$ | $O(1)$ |
| **Average Case** | $O(n^2)$ | $O(1)$ |
| **Worst Case** | $O(n^2)$ | $O(1)$ |

- **Stable**: Generally No (swapping can change the relative order of equal elements).
- **In-place**: Yes.

## Example and Dry Run
**Input**: `[64, 25, 12, 22, 11]`

**Iteration 1**:
- Unsorted: `[64, 25, 12, 22, 11]`
- Min found: `11` (index 4)
- Swap `64` and `11`
- Result: `[11, 25, 12, 22, 64]`

**Iteration 2**:
- Unsorted (from index 1): `[25, 12, 22, 64]`
- Min found: `12` (index 2)
- Swap `25` and `12`
- Result: `[11, 12, 25, 22, 64]`

**Iteration 3**:
- Unsorted (from index 2): `[25, 22, 64]`
- Min found: `22` (index 3)
- Swap `25` and `22`
- Result: `[11, 12, 22, 25, 64]`

**Iteration 4**:
- Unsorted (from index 3): `[25, 64]`
- Min found: `25` (index 3)
- No swap needed.
- Result: `[11, 12, 22, 25, 64]`
*Array is Sorted.*

## Variations
1. **Stable Selection Sort**: Instead of swapping, minimum element is inserted into position by shifting elements.
2. **Double Selection Sort (Bidirectional)**: Find both minimum and maximum in each pass and move them to their respective ends.
