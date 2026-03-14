---
title: 05 - Ternary Search
tags:
  - Searching
  - Divide-and-Conquer
---

## Problem
Search for a target element in a **sorted** array by dividing the search interval into three equal parts instead of two.

## Intuition
Binary Search divides the array into two halves. Ternary Search takes it a step further by dividing the search space into three parts using two midpoints (`mid1` and `mid2`). 

Think of it like looking for a house on a street where you have two scouts. Scout A checks the house at 1/3rd of the street, and Scout B checks the house at 2/3rds. Depending on where the target is relative to these two points, you can eliminate 2/3rds of the street in a single step (if lucky) or 1/3rd in the worst case.

## Algorithm Steps
1. Initialize `low = 0` and `high = n - 1`.
2. While `low <= high`:
   - Calculate two midpoints:
     - `mid1 = low + (high - low) / 3`
     - `mid2 = high - (high - low) / 3`
   - If `A[mid1] == target`, return `mid1`.
   - If `A[mid2] == target`, return `mid2`.
   - If `target < A[mid1]`, search in the first part: `high = mid1 - 1`.
   - If `target > A[mid2]`, search in the third part: `low = mid2 + 1`.
   - Otherwise, search in the middle part: `low = mid1 + 1`, `high = mid2 - 1`.
3. If not found, return -1.

## Pseudocode
```text
procedure ternarySearch(A, low, high, target)
    if high >= low then
        mid1 := low + (high - low) / 3
        mid2 := high - (high - low) / 3
        
        if A[mid1] == target then return mid1
        if A[mid2] == target then return mid2
        
        if target < A[mid1] then
            return ternarySearch(A, low, mid1 - 1, target)
        else if target > A[mid2] then
            return ternarySearch(A, mid2 + 1, high, target)
        else
            return ternarySearch(A, mid1 + 1, mid2 - 1, target)
    end if
    return -1
end procedure
```

## Java Code using JCF
Using a competitive programming structure (TCS NQT Style).

```java
import java.util.*;

public class Main {
    /**
     * Ternary Search Implementation (Iterative)
     */
    public static int ternarySearch(List<Integer> list, int target) {
        int low = 0, high = list.size() - 1;

        while (low <= high) {
            int mid1 = low + (high - low) / 3;
            int mid2 = high - (high - low) / 3;

            if (list.get(mid1) == target) return mid1;
            if (list.get(mid2) == target) return mid2;

            if (target < list.get(mid1)) {
                high = mid1 - 1;
            } else if (target > list.get(mid2)) {
                low = mid2 + 1;
            } else {
                low = mid1 + 1;
                high = mid2 - 1;
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
                        System.out.println(ternarySearch(list, target));
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
9
1 2 3 4 5 6 7 8 9
5
5
10 20 30 40 50
25
```

**Output:**
```text
4
-1
```

## Complexity
| Case | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **All Cases** | $O(\log_3 n)$ | $O(1)$ (Iterative) / $O(\log_3 n)$ (Recursive) |

- **Comparison to Binary Search**: Ternary search performs more comparisons per iteration than binary search. Even though it has a better logarithmic base ($\log_3$ vs $\log_2$), the extra comparisons usually make it slower in practice on actual computers.
- **Sorted Required**: **Yes**.

## Example and Dry Run
**Input**: `list = [1, 2, 3, 4, 5, 6, 7, 8, 9]`, `target = 5`
- `low=0, high=8`

1. `mid1 = 0 + (8-0)/3 = 2`. `list[2] = 3`.
2. `mid2 = 8 - (8-0)/3 = 6`. `list[6] = 7`.
3. `5` is between `3` and `7`.
4. `low = 2+1 = 3`, `high = 6-1 = 5`.
5. New range `[3, 5]`. `mid1 = 3 + (5-3)/3 = 3`. `list[3] = 4`.
6. `mid2 = 5 - (5-3)/3 = 5`. `list[5] = 6`.
7. `5` is between `4` and `6`.
8. `low = 3+1 = 4`, `high = 5-1 = 4`.
9. `mid1 = 4, mid2 = 4`. `list[4] = 5`. **Found!** Return 4.

## Variations
1. **Unimodal Function Optimization**: Ternary search is frequently used to find the maximum or minimum of a unimodal function (a function that increases then decreases, or vice versa) where derivatives might be hard to calculate.
