---
title: 18 - Search in Rotated Sorted Array
tags:
  - Array
  - Binary-Search
  - Medium
---

You are given a rotated sorted array `nums` of unique integers and a `target` integer. Return the index of `target` if it is in `nums`, or `-1` if it is not in `nums`.

You must write an algorithm that runs in $O(\log n)$ time.

---

## Examples

**Example 1**

Input: 
```
nums = [3, 4, 5, 6, 1, 2], target = 1
```

Output: 
```
4
```

---

**Example 2**

Input: 
```
nums = [3, 5, 6, 0, 1, 2], target = 4
```

Output: 
```
-1
```

---

## Solution

### Java Code

```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {
            int mid = left + (right - left) / 2;

            if (nums[mid] == target) {
                return mid;
            }

            // Determine which half is sorted
            if (nums[left] <= nums[mid]) {
                // Left half is sorted
                if (target >= nums[left] && target < nums[mid]) {
                    // Target is within the sorted left half
                    right = mid - 1;
                } else {
                    // Target is in the other half
                    left = mid + 1;
                }
            } else {
                // Right half is sorted
                if (target > nums[mid] && target <= nums[right]) {
                    // Target is within the sorted right half
                    left = mid + 1;
                } else {
                    // Target is in the other half
                    right = mid - 1;
                }
            }
        }

        return -1;
    }
}
```

---

## Java Utility Components Used

Technically, this solution uses standard **Binary Search** logic without specific external JCF classes beyond basic array indexing, but it follows the pattern of efficient searching required by the JCF's `Arrays.binarySearch()` (though a standard binary search won't work directly on a rotated array).

---

## Intuition

Even though the array is rotated, at least one half of the divided array (separated by `mid`) will always be sorted.

1. **Identify Sorted Half**: Check if `nums[left] <= nums[mid]`.
   - If true, the **left** half is sorted.
   - If false, the **right** half must be sorted.
2. **Target Check**: 
   - If the sorted half contains the `target` (based on its range), search there.
   - Otherwise, search the other half.
3. **Repeat**: Continue splitting until the target is found or the pointers cross.

This refined Binary Search adapts to the "pivot" point while maintaining logarithmic time complexity.

---

## Complexity Analysis

### Time Complexity

```
O(log n)
```

Standard Binary Search performance as we eliminate half of the remaining search space in each step.

---

### Space Complexity

```
O(1)
```

Only three integer variables (`left`, `right`, `mid`) are used.

---

## Key Takeaways

- **Pivot Identification**: In any rotated sorted array, at least one half of a window is always sorted.
- **Range Verification**: Use the sorted half to clearly determine if a value can exist within it.
- Logarithmic search is mandatory for performance on large rotated datasets.
