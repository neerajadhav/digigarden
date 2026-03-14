---
title: 06 - Heap Sort
tags:
  - Sorting
  - Heap
---

## Problem
Sort an array by first building a Max-Heap and then repeatedly extracting the maximum element from the heap and placing it at the end of the array.

## Intuition
Heap Sort is a comparison-based sorting technique based on a **Binary Heap** data structure. It's similar to selection sort where we first find the maximum element and place it at the end. However, instead of scanning the entire unsorted part to find the maximum, we use a Heap to find it in $O(\log n)$ time.

Think of it as maintaining a "priority" structure where the largest item is always at the root. You take the root, swap it with the last leaf, "repair" the heap, and repeat.

## Algorithm Steps
1. **Build a Max-Heap**: Transform the input array into a max-heap.
2. **Sort**:
   - Swap the root (maximum element) with the last element of the heap.
   - Reduce the heap size by 1.
   - **Heapify** the root of the tree to maintain the max-heap property.
   - Repeat until the heap size is 1.

## Pseudocode
```text
procedure heapSort(A)
    buildMaxHeap(A)
    for i := length(A) - 1 down to 1 do
        swap(A[0], A[i])
        heapify(A, 0, i)
    end for
end procedure

procedure heapify(A, i, n)
    largest := i
    l := 2*i + 1
    r := 2*i + 2
    
    if l < n and A[l] > A[largest] then largest := l
    if r < n and A[r] > A[largest] then largest := r
    
    if largest != i then
        swap(A[i], A[largest])
        heapify(A, largest, n)
    end if
end procedure
```

## Java Code using JCF
Using a competitive programming structure (TCS NQT Style). While `PriorityQueue` exists in JCF, Heap Sort is usually implemented manually for $O(1)$ space.

```java
import java.util.*;

public class Main {
    /**
     * Heap Sort Function
     */
    public static void heapSort(List<Integer> list) {
        int n = list.size();

        // Build heap (rearrange list)
        for (int i = n / 2 - 1; i >= 0; i--) {
            heapify(list, n, i);
        }

        // One by one extract an element from heap
        for (int i = n - 1; i > 0; i--) {
            // Move current root to end
            Collections.swap(list, 0, i);

            // call max heapify on the reduced heap
            heapify(list, i, 0);
        }
    }

    /**
     * To heapify a subtree rooted with node i which is an index in list
     */
    private static void heapify(List<Integer> list, int n, int i) {
        int largest = i; // Initialize largest as root
        int l = 2 * i + 1; // left = 2*i + 1
        int r = 2 * i + 2; // right = 2*i + 2

        // If left child is larger than root
        if (l < n && list.get(l) > list.get(largest))
            largest = l;

        // If right child is larger than largest so far
        if (r < n && list.get(r) > list.get(largest))
            largest = r;

        // If largest is not root
        if (largest != i) {
            Collections.swap(list, i, largest);

            // Recursively heapify the affected sub-tree
            heapify(list, n, largest);
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
                    
                    heapSort(list);
                    
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
4 10 3 5 1
```

**Output:**
```text
5 6 7 11 12 13
1 3 4 5 10
```

## Complexity
| Case | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **Best Case** | $O(n \log n)$ | $O(1)$ |
| **Average Case** | $O(n \log n)$ | $O(1)$ |
| **Worst Case** | $O(n \log n)$ | $O(1)$ |

- **Stable**: No.
- **In-place**: Yes.
- **Comparison-based**: Yes.

## Example and Dry Run
**Input**: `[4, 10, 3, 5, 1]`

1. **Build Max-Heap**:
   - Heapify starting from indices $n/2-1$ (index 1).
   - Index 1 (10): Children are 5, 1. Max is 10. No change.
   - Index 0 (4): Children are 10, 3. Max is 10. Swap 4 and 10.
   - Heapified array: `[10, 4, 3, 5, 1]`
   - Continue heapifying index 1 (now 4): Children are 5, 1. Max is 5. Swap 4 and 5.
   - Final Max-Heap: `[10, 5, 3, 4, 1]`

2. **Sorting Phase**:
   - Swap root (10) with last (1) $\rightarrow$ `[1, 5, 3, 4, 10]`. Heapify root.
   - Swap root (5) with last (4) $\rightarrow$ `[4, 3, 1, 5, 10]`. Heapify root.
   - Swap root (4) with last (1) $\rightarrow$ `[1, 3, 4, 5, 10]`. Heapify root.
   - Swap root (3) with last (1) $\rightarrow$ `[1, 3, 4, 5, 10]`.
- **Result**: `[1, 3, 4, 5, 10]`

## Variations
1. **Min-Heap Sort**: Instead of a max-heap, build a min-heap to sort the array in descending order.
2. **PriorityQueue Sort**: In Java, you can simply add all elements to a `PriorityQueue` and poll them out. This is $O(n \log n)$ time but $O(n)$ space.
3. **Smoothsort**: A variation of heap sort that is $O(n)$ in the best case (already sorted).
