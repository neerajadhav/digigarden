---
title: 05 - Quick Sort
tags:
  - Sorting
---

## Problem
Sort an array by picking an element as a "pivot" and partitioning the given array around the picked pivot.

## Intuition
Quick Sort is a **Divide and Conquer** algorithm. It works by selecting a 'pivot' element from the array and partitioning the other elements into two sub-arrays, according to whether they are less than or greater than the pivot. The sub-arrays are then sorted recursively.

Think of it like organizing a group of people by height. Pick one person (the pivot), and have everyone shorter stand to their left and everyone taller stand to their right. Now the pivot is in their final sorted position. Repeat this for the left and right groups.

## Algorithm Steps
1. **Pick a pivot**: Direct choice (last element, first element, median).
2. **Partition**: Rearrange the array so that elements smaller than the pivot are on the left, and elements larger are on the right.
3. **Recursively apply**: Repeat the process for the left and right sub-arrays.

## Pseudocode
```text
procedure quickSort(A, low, high)
    if low < high then
        pi := partition(A, low, high)
        quickSort(A, low, pi - 1)
        quickSort(A, pi + 1, high)
    end if
end procedure

procedure partition(A, low, high)
    pivot := A[high]
    i := low - 1
    for j := low to high - 1 do
        if A[j] < pivot then
            i := i + 1
            swap(A[i], A[j])
        end if
    end for
    swap(A[i + 1], A[high])
    return i + 1
end procedure
```

## Java Code using JCF
Using a competitive programming structure (TCS NQT Style).

```java
import java.util.*;

public class Main {
    /**
     * Quick Sort Main Function
     */
    public static void quickSort(List<Integer> list, int low, int high) {
        if (low < high) {
            // pi is partitioning index, list.get(pi) is now at right place
            int pi = partition(list, low, high);

            // Recursively sort elements before partition and after partition
            quickSort(list, low, pi - 1);
            quickSort(list, pi + 1, high);
        }
    }

    /**
     * Helper function to partition the list using the last element as pivot
     */
    private static int partition(List<Integer> list, int low, int high) {
        int pivot = list.get(high);
        int i = (low - 1); // index of smaller element

        for (int j = low; j < high; j++) {
            // If current element is smaller than the pivot
            if (list.get(j) < pivot) {
                i++;
                Collections.swap(list, i, j);
            }
        }

        // Swap the pivot element with the element at i+1
        Collections.swap(list, i + 1, high);
        return i + 1;
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
                    
                    quickSort(list, 0, list.size() - 1);
                    
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
6
10 7 8 9 1 5
5
1 2 3 4 5
```

**Output:**
```text
1 5 7 8 9 10
1 2 3 4 5
```

## Complexity
| Case | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **Best Case** | $O(n \log n)$ | $O(\log n)$ |
| **Average Case** | $O(n \log n)$ | $O(\log n)$ |
| **Worst Case** | $O(n^2)$ | $O(n)$ |

- **Stable**: No.
- **In-place**: Yes.
- **Note**: Worst case occurs when the pivot is always the smallest or largest element (e.g., sorted array with last element as pivot).

## Example and Dry Run
**Input**: `[10, 7, 8, 9, 1, 5]`

1. **Pivot**: 5
2. **Partitioning**:
   - 1 < 5, swap with 10 $\rightarrow$ `[1, 7, 8, 9, 10, 5]`
   - End of loop, swap 5 with 7 $\rightarrow$ `[1, 5, 8, 9, 10, 7]`
   - Pivot 5 is now at index 1.
3. **Left of 5**: `[1]` (Sorted)
4. **Right of 5**: `[8, 9, 10, 7]`
   - Pivot: 7
   - Partition: All elements > 7. Swap 7 with 8 $\rightarrow$ `[7, 9, 10, 8]`
   - Pivot 7 is now at index 2.
5. **Right of 7**: `[9, 10, 8]`
   - Continue until sorted...
- **Result**: `[1, 5, 7, 8, 9, 10]`

## Variations
1. **Randomized Quick Sort**: Choose a random element as pivot to avoid the $O(n^2)$ worst-case on sorted data.
2. **3-Way Quick Sort (Dutch National Flag Algorithm)**: Handles arrays with many duplicate elements efficiently by partitioning into three parts: less than, equal to, and greater than the pivot.
3. **Introsort**: A hybrid algorithm that starts with Quick Sort and switches to Heap Sort if the recursion depth exceeds a certain level.
