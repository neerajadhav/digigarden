---
title: 04 - Interpolation Search
tags:
  - Searching
  - Optimization
---

## Problem
An optimized search for **sorted** arrays where elements are **uniformly distributed**. It estimates the position of the target element based on its value relative to the range of values in the current interval.

## Intuition
Think of looking for a name in a phone book. If the name starts with "A", you don't start in the middle (like Binary Search); you start near the beginning. Interpolation Search uses a mathematical formula to guess where the target might be, similar to how we humanly search in a sorted, uniform list.

## Algorithm Steps
1. The position formula:
   $pos = low + \left[ \frac{(target - A[low]) \cdot (high - low)}{A[high] - A[low]} \right]$
2. In each step:
   - Calculate `pos`.
   - If `A[pos] == target`, return `pos`.
   - If `target < A[pos]`, target is in the left sub-array (set `high = pos - 1`).
   - If `target > A[pos]`, target is in the right sub-array (set `low = pos + 1`).
3. Repeat while `low <= high` and `target` is within the range `[A[low], A[high]]`.

## Pseudocode
```text
procedure interpolationSearch(A, target, n)
    low := 0
    high := n - 1
    while low <= high and target >= A[low] and target <= A[high] do
        if low == high then
            if A[low] == target then return low
            return -1
        end if
        
        pos := low + (((target - A[low]) * (high - low)) / (A[high] - A[low]))
        
        if A[pos] == target then return pos
        if A[pos] < target then low := pos + 1
        else high := pos - 1
    end while
    return -1
end procedure
```

## Java Code using JCF
Using a competitive programming structure (TCS NQT Style).

```java
import java.util.*;

public class Main {
    /**
     * Interpolation Search Implementation
     */
    public static int interpolationSearch(List<Integer> list, int target) {
        int low = 0, high = list.size() - 1;

        while (low <= high && target >= list.get(low) && target <= list.get(high)) {
            if (low == high) {
                if (list.get(low) == target) return low;
                return -1;
            }

            // Estimate position
            // Casting to long to prevent overflow in (target - list.get(low)) * (high - low)
            int pos = low + (int)(( (long)(target - list.get(low)) * (high - low) ) / (list.get(high) - list.get(low)));

            if (list.get(pos) == target) {
                return pos;
            }

            if (list.get(pos) < target) {
                low = pos + 1;
            } else {
                high = pos - 1;
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
                    Collections.sort(list);
                    if (sc.hasNextInt()) {
                        int target = sc.nextInt();
                        System.out.println(interpolationSearch(list, target));
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
1
10
10 12 13 16 18 19 20 21 22 23
18
```

**Output:**
```text
4
```

## Complexity
| Case | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **Best Case** | $O(1)$ | $O(1)$ |
| **Average Case** | $O(\log \log n)$ | $O(1)$ |
| **Worst Case** | $O(n)$ (Exponentially distributed values) | $O(1)$ |

- **Requirement**: Sorted array + Uniformly distributed values.
- **Advantage**: Significantly faster than Binary Search for large, uniform data.

## Example and Dry Run
**Input**: `list = [10, 12, 13, 16, 18, 19, 20, 21, 22, 23]`, `target = 18`
- `low=0, high=9, A[low]=10, A[high]=23`

1. `pos = 0 + [ (18-10) * (9-0) ] / (23-10)`
2. `pos = 0 + (8 * 9) / 13 = 72 / 13 = 5.53` $\rightarrow$ index 5.
3. `list[5] = 19`. `18 < 19`, so `high = 4`.
4. New range `[0, 4]`, `A[low]=10, A[high]=18`.
5. `pos = 0 + [ (18-10) * (4-0) ] / (18-10) = 4`.
6. `list[4] = 18`. **Found!** Return 4.

## Variations
1. **Extrapolated Search**: Similar logic but for different data distributions.
