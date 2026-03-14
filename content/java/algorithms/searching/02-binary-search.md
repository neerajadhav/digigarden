---
title: 02 - Binary Search
tags:
  - Searching
  - Divide-and-Conquer
---

# Binary Search

## Problem
Search for a target element in a **sorted** array by repeatedly dividing the search interval in half.

## Intuition
Binary search is much faster than linear search for large datasets. Think of looking for a word in a physical dictionary. You don't start from the first page; you open it in the middle. If the word you are looking for comes after the middle page, you ignore the first half and repeat the process for the second half.

## Algorithm Steps
1. Initialize `low = 0` and `high = n - 1`.
2. While `low <= high`:
   - Calculate the middle index: `mid = low + (high - low) / 2`.
   - If `A[mid] == target`, return `mid`.
   - If `target < A[mid]`, target must be in the left half, so set `high = mid - 1`.
   - If `target > A[mid]`, target must be in the right half, so set `low = mid + 1`.
3. If the loop ends without finding the target, return -1.

## Pseudocode
```text
procedure binarySearch(A, target)
    low := 0
    high := length(A) - 1
    while low <= high do
        mid := low + (high - low) / 2
        if A[mid] == target then
            return mid
        else if A[mid] > target then
            high := mid - 1
        else
            low := mid + 1
    end while
    return -1
end procedure
```

## Java Code using JCF
Using a competitive programing structure (TCS NQT Style).

```java
import java.util.*;

public class Main {
    /**
     * Iterative Binary Search on a List
     */
    public static int binarySearch(List<Integer> list, int target) {
        int low = 0, high = list.size() - 1;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (list.get(mid) == target) {
                return mid;
            }

            if (target < list.get(mid)) {
                high = mid - 1;
            } else {
                low = mid + 1;
            }
        }
        return -1;
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
                    
                    // Arrays must be sorted for Binary Search
                    Collections.sort(list);
                    
                    if (sc.hasNextInt()) {
                        int target = sc.nextInt();
                        int result = binarySearch(list, target);
                        System.out.println(result);
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
5
10 20 30 40 50
30
4
1 5 7 10
2
```

**Output:**
```text
2
-1
```

## Complexity
| Case | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **Best Case** | $O(1)$ (Target is the middle element) | $O(1)$ |
| **Average Case** | $O(\log n)$ | $O(1)$ |
| **Worst Case** | $O(\log n)$ | $O(1)$ |

- **Sorted Required**: **Yes**.
- **Strategy**: Divide and Conquer.

## Example and Dry Run
**Input**: `list = [10, 20, 30, 40, 50]`, `target = 30`

1. `low = 0, high = 4, mid = 2`. `list[2] = 30`. **Found!** Return 2.

**Input**: `list = [1, 5, 7, 10]`, `target = 2`

1. `low = 0, high = 3, mid = 1`. `list[1] = 5`. `2 < 5`, so `high = 0`.
2. `low = 0, high = 0, mid = 0`. `list[0] = 1`. `2 > 1`, so `low = 1`.
3. `low = 1, high = 0`. Loop ends. Return -1.

## Variations
1. **Recursive Binary Search**: Using recursion to achieve the same result.
2. **First/Last Occurrence**: Modification to find the first or last instance of a duplicate target.
3. **Bisect (Python Style)**: Finding the insertion point to maintain order.
