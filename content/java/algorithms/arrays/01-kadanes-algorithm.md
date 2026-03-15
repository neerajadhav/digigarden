---
title: 01 - Kadane's Algorithm
tags:
  - Arrays
  - Dynamic Programming
  - Greedy
---

## Problem
Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

## Intuition
Kadane's algorithm is a greedy/dynamic programming approach. The core idea is that the maximum subarray sum ending at the current position is either the current element itself or the current element plus the maximum subarray sum ending at the previous position. We maintain a running sum and update the global maximum whenever the running sum exceeds it. If the running sum becomes negative, we reset it to zero (or the current element) because a negative prefix will only decrease the sum of any subsequent subarray.

## Algorithm Steps
1. Initialize `maxSoFar` with a very small value (or the first element).
2. Initialize `currentMax` with 0 (or the first element).
3. Iterate through each element of the array:
    a. Add the current element to `currentMax`.
    b. If `currentMax > maxSoFar`, update `maxSoFar = currentMax`.
    c. If `currentMax < 0`, reset `currentMax = 0`.
4. Return `maxSoFar`.

## Pseudocode
```text
procedure kadanesAlgorithm(A)
    maxSoFar := -infinity
    currentMax := 0
    for each x in A do
        currentMax := currentMax + x
        if maxSoFar < currentMax then
            maxSoFar := currentMax
        end if
        if currentMax < 0 then
            currentMax := 0
        end if
    end for
    return maxSoFar
end procedure
```

## Java Code using JCF
Using a competitive programming structure.

```java
import java.util.*;

public class Main {
    /**
     * Kadane's Algorithm to find maximum subarray sum
     */
    public static long kadanes(List<Integer> nums) {
        long maxSoFar = Long.MIN_VALUE;
        long currentMax = 0;

        for (int x : nums) {
            currentMax += x;
            if (maxSoFar < currentMax) {
                maxSoFar = currentMax;
            }
            if (currentMax < 0) {
                currentMax = 0;
            }
        }
        return maxSoFar;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        if (sc.hasNextInt()) {
            int t = sc.nextInt();
            while (t-- > 0) {
                if (sc.hasNextInt()) {
                    int n = sc.nextInt();
                    List<Integer> nums = new ArrayList<>();
                    for (int i = 0; i < n; i++) {
                        if (sc.hasNextInt()) {
                            nums.add(sc.nextInt());
                        }
                    }
                    
                    long result = kadanes(nums);
                    System.out.println(result);
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
-2 1 -3 4 -1 2 1 -5 4
5
1 2 3 -2 5
```

**Output:**
```text
6
9
```

## Complexity
| Case | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **Best Case** | $O(n)$ | $O(1)$ |
| **Average Case** | $O(n)$ | $O(1)$ |
| **Worst Case** | $O(n)$ | $O(1)$ |

- **Type**: Iterative / Greedy.
- **In-place**: Yes.
- **Stable**: N/A (Searching/Summing).

## Example and Dry Run
**Input**: `nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]`

1. **x = -2**: `currentMax = -2`. `maxSoFar = -2`. `currentMax < 0` $\rightarrow$ `currentMax = 0`.
2. **x = 1**: `currentMax = 1`. `maxSoFar = 1`.
3. **x = -3**: `currentMax = -2`. `maxSoFar = 1`. `currentMax < 0` $\rightarrow$ `currentMax = 0`.
4. **x = 4**: `currentMax = 4`. `maxSoFar = 4`.
5. **x = -1**: `currentMax = 3`. `maxSoFar = 4`.
6. **x = 2**: `currentMax = 5`. `maxSoFar = 5`.
7. **x = 1**: `currentMax = 6`. `maxSoFar = 6`.
8. **x = -5**: `currentMax = 1`. `maxSoFar = 6`.
9. **x = 4**: `currentMax = 5`. `maxSoFar = 6`.

**Final Result**: 6 (Subarray: `[4, -1, 2, 1]`)

## Variations
1. **Printing Subarray**: Maintain `start`, `end`, and `tempStart` pointers to track the actual indices of the maximum subarray.
2. **Smallest Subarray Sum**: Invert the logic (or multiply all elements by -1) to find the minimum contiguous sum.
3. **Circular Subarray Sum**: Find `max(Kadane(A), TotalSum - KadaneMin(A))`.
