---
title: 04 - Two Pointer
tags:
  - Arrays
  - Searching
  - Fundamentals
---

## Problem
Given a sorted array of integers `nums` and a target sum `target`, find if there exists a pair of elements that sum up to the target.

## Intuition
The two-pointer technique is a powerful optimization for problems involving searching in a sorted data structure. Instead of using nested loops ($O(n^2)$), we initialize two pointers: one at the beginning (`left`) and one at the end (`right`). We then move these pointers towards each other based on the current sum relative to the target. This reduces the complexity to $O(n)$.

## Algorithm Steps
1. Initialize `left = 0` and `right = nums.size() - 1`.
2. While `left < right`:
    a. Calculate `currentSum = nums[left] + nums[right]`.
    b. If `currentSum == target`:
        - Return `true` (or the indices).
    c. If `currentSum < target`:
        - We need a larger sum, so move the `left` pointer forward (`left++`).
    d. If `currentSum > target`:
        - We need a smaller sum, so move the `right` pointer backward (`right--`).
3. If the loop ends without finding a pair, return `false`.

## Pseudocode
```text
procedure twoPointer(A, target)
    left := 0
    right := length(A) - 1
    while left < right do
        sum := A[left] + A[right]
        if sum == target then
            return true
        else if sum < target then
            left := left + 1
        else
            right := right - 1
        end if
    end while
    return false
end procedure
```

## Java Code using JCF
Using a competitive programming structure.

```java
import java.util.*;

public class Main {
    /**
     * Two Pointer technique to find pair with target sum
     */
    public static boolean hasPairWithSum(List<Integer> nums, int target) {
        int left = 0;
        int right = nums.size() - 1;

        while (left < right) {
            int currentSum = nums.get(left) + nums.get(right);
            if (currentSum == target) {
                return true;
            } else if (currentSum < target) {
                left++;
            } else {
                right--;
            }
        }
        return false;
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
                        int target = sc.nextInt();
                        boolean found = hasPairWithSum(nums, target);
                        System.out.println(found ? "YES" : "NO");
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
1 2 3 4 6
6
3
2 5 9
11
```

**Output:**
```text
YES
YES
```

## Complexity
| Case | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **All Cases** | $O(n)$ | $O(1)$ |

- **Type**: Iterative / Two Pointers.
- **Sorted Required**: Yes.
- **In-place**: Yes.

## Example and Dry Run
**Input**: `nums = [1, 2, 3, 4, 6]`, `target = 6`

1. **left = 0, right = 4**: `nums[0] + nums[4] = 1 + 6 = 7`. `7 > 6` $\rightarrow$ `right--`.
2. **left = 0, right = 3**: `nums[0] + nums[3] = 1 + 4 = 5`. `5 < 6` $\rightarrow$ `left++`.
3. **left = 1, right = 3**: `nums[1] + nums[3] = 2 + 4 = 6`. `6 == 6`. **Found!** Return `true`.

**Final Result**: YES

## Variations
1. **Squaring a Sorted Array**: Using two pointers at both ends to fill a new array from largest to smallest square.
2. **Triplet Sum to Zero**: Sort the array, then iterate and use two pointers for the remaining pair sum.
3. **Container With Most Water**: Two pointers moving inwards to maximize the area.
4. **Remove Duplicates**: One "slow" pointer and one "fast" pointer to modify the array in-place.
