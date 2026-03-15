---
title: 02 - Sliding Window
tags:
  - Arrays
  - Two Pointers
  - Fundamentals
---

## Problem
Given an array of integers `nums` and a number `k`, find the maximum sum of any contiguous subarray of size `k`.

## Intuition
The sliding window technique is used to perform a required operation on a specific window size of a given buffer (array or string). Instead of recalculating the sum for every subarray from scratch ($O(n \cdot k)$), we "slide" the window by adding the next element and removing the first element of the previous window. This keeps the time complexity at $O(n)$.

## Algorithm Steps
1. Calculate the sum of the first `k` elements. This is our initial `windowSum`.
2. Initialize `maxSum` with `windowSum`.
3. Iterate from the `k`-th element to the end of the array:
    a. Update `windowSum` by adding the current element and subtracting the element that is no longer in the window (at index `i - k`).
    b. Update `maxSum = max(maxSum, windowSum)`.
4. Return `maxSum`.

## Pseudocode
```text
procedure slidingWindow(A, k)
    windowSum := sum of first k elements
    maxSum := windowSum
    for i := k to length(A) - 1 do
        windowSum := windowSum + A[i] - A[i - k]
        maxSum := max(maxSum, windowSum)
    end for
    return maxSum
end procedure
```

## Java Code using JCF
Using a competitive programming structure.

```java
import java.util.*;

public class Main {
    /**
     * Fixed-size Sliding Window to find maximum sum of subarray of size K
     */
    public static long findMaxSum(List<Integer> nums, int k) {
        if (nums == null || nums.size() < k || k <= 0) {
            return 0;
        }

        long windowSum = 0;
        for (int i = 0; i < k; i++) {
            windowSum += nums.get(i);
        }

        long maxSum = windowSum;

        for (int i = k; i < nums.size(); i++) {
            windowSum += nums.get(i) - nums.get(i - k);
            maxSum = Math.max(maxSum, windowSum);
        }

        return maxSum;
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
                    
                    if (sc.hasNextInt()) {
                        int k = sc.nextInt();
                        long result = findMaxSum(nums, k);
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
6
2 1 5 1 3 2
3
7
2 3 4 1 5 6 2
4
```

**Output:**
```text
9
16
```

## Complexity
| Case | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **All Cases** | $O(n)$ | $O(1)$ |

- **Type**: Iterative / Sliding Window.
- **In-place**: Yes.
- **Sorted Required**: No.

## Example and Dry Run
**Input**: `nums = [2, 1, 5, 1, 3, 2]`, `k = 3`

1. **Initial Window (0-2)**: `2 + 1 + 5 = 8`. `windowSum = 8`, `maxSum = 8`.
2. **Slide to index 3**: `windowSum = 8 + 1 - 2 = 7`. `maxSum = max(8, 7) = 8`.
3. **Slide to index 4**: `windowSum = 7 + 3 - 1 = 9`. `maxSum = max(8, 9) = 9`.
4. **Slide to index 5**: `windowSum = 9 + 2 - 5 = 6`. `maxSum = max(9, 6) = 9`.

**Final Result**: 9

## Variations
1. **Variable Size Window**: Finding the smallest subarray with a sum greater than or equal to `S`.
2. **Longest Substring with K Distinct Characters**: Using a map to track character frequencies within the window.
3. **Sliding Window Maximum**: Find the maximum element in each window of size `K` (requires a Deque).
