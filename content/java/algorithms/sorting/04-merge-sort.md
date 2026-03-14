---
title: 04 - Merge Sort
tags:
  - Sorting
---

## Problem
Sort an array by using the Divide and Conquer strategy to recursively divide the problem into smaller sub-problems until they are simple enough to be solved directly.

## Intuition
Merge Sort is based on the idea of **Divide and Conquer**. Imagine you have two sorted piles of cards. It's very easy to merge them into one larger sorted pile: you just compare the top cards of each pile and pick the smaller one. Merge Sort does exactly this by recursively splitting the array in half until each piece has only one element (which is already sorted) and then merging them back together.

## Algorithm Steps
1. **Divide**: Find the middle point of the array and divide it into two halves.
2. **Conquer**: Recursively call merge sort for the left half and the right half.
3. **Combine**: Merge the two sorted halves into a single sorted array.

## Pseudocode
```text
procedure mergeSort(A : list, l : left index, r : right index)
    if l < r then
        m := l + (r - l) / 2
        mergeSort(A, l, m)
        mergeSort(A, m + 1, r)
        merge(A, l, m, r)
    end if
end procedure

procedure merge(A, l, m, r)
    n1 := m - l + 1
    n2 := r - m
    create arrays L[n1] and R[n2]
    copy A[l..m] to L and A[m+1..r] to R
    
    i := 0, j := 0, k := l
    while i < n1 and j < n2 do
        if L[i] <= R[j] then
            A[k] := L[i]
            i := i + 1
        else
            A[k] := R[j]
            j := j + 1
        end if
        k := k + 1
    end while
    
    copy remaining elements of L and R
end procedure
```

## Java Code using JCF
Using `ArrayList` with a competitive programming structure (TCS NQT Style).

```java
import java.util.*;

public class Main {
    /**
     * Merge Sort Main Function
     */
    public static void mergeSort(List<Integer> list, int left, int right) {
        if (left < right) {
            int mid = left + (right - left) / 2;

            // Sort first and second halves
            mergeSort(list, left, mid);
            mergeSort(list, mid + 1, right);

            // Merge the sorted halves
            merge(list, left, mid, right);
        }
    }

    /**
     * Helper Function to Merge two sorted portions
     */
    private static void merge(List<Integer> list, int left, int mid, int right) {
        int n1 = mid - left + 1;
        int n2 = right - mid;

        List<Integer> L = new ArrayList<>(n1);
        List<Integer> R = new ArrayList<>(n2);

        for (int i = 0; i < n1; ++i) L.add(list.get(left + i));
        for (int j = 0; j < n2; ++j) R.add(list.get(mid + 1 + j));

        int i = 0, j = 0, k = left;
        while (i < n1 && j < n2) {
            if (L.get(i) <= R.get(j)) {
                list.set(k++, L.get(i++));
            } else {
                list.set(k++, R.get(j++));
            }
        }

        while (i < n1) list.set(k++, L.get(i++));
        while (j < n2) list.set(k++, R.get(j++));
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
                    
                    mergeSort(list, 0, list.size() - 1);
                    
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
12 11 13 5 6 7
5
38 27 43 3 9
```

**Output:**
```text
5 6 7 11 12 13
3 9 27 38 43
```

## Complexity
| Case | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **Best Case** | $O(n \log n)$ | $O(n)$ |
| **Average Case** | $O(n \log n)$ | $O(n)$ |
| **Worst Case** | $O(n \log n)$ | $O(n)$ |

- **Stable**: Yes.
- **In-place**: No ($O(n)$ extra space for merging).
- **Strategy**: Divide and Conquer.

## Example and Dry Run
**Input**: `[38, 27, 43, 3, 9]`

1. **Split**: `[38, 27, 43]` and `[3, 9]`
2. **Split Left**: `[38]` and `[27, 43]`
3. **Split Further**: `[27]` and `[43]`
4. **Merge `[27], [43]`**: `[27, 43]`
5. **Merge `[38], [27, 43]`**: `[27, 38, 43]`
6. **Merge Right `[3], [9]`**: `[3, 9]`
7. **Final Merge `[27, 38, 43]` and `[3, 9]`**: 
   - Compare 27 and 3 $\rightarrow$ 3
   - Compare 27 and 9 $\rightarrow$ 9
   - Append remaining `[27, 38, 43]`
- **Result**: `[3, 9, 27, 38, 43]`

## Variations
1. **Iterative Merge Sort (Bottom-up)**: Avoids recursion by merging small subarrays into larger ones iteratively.
2. **In-place Merge Sort**: Aims to reduce space complexity to $O(1)$, though usually at the cost of time ($O(n^2)$ or worse) or stability.
3. **Timsort**: A hybrid sorting algorithm derived from merge sort and insertion sort, used in Java's `Arrays.sort()`.
