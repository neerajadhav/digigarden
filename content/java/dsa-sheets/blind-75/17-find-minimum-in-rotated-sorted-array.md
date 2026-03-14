---
title: 17 - Find Minimum in Rotated Sorted Array
tags:
  - Array
  - Binary-Search
  - Medium
---

You are given an array of length `n` which was originally sorted in ascending order. It has now been rotated between 1 and `n` times. 

Assuming all elements in the rotated sorted array `nums` are unique, return the **minimum element** of this array.

You must write an algorithm that runs in $O(\log n)$ time.

---

## Examples

**Example 1**

Input: 
```
nums = [3, 4, 5, 6, 1, 2]
```

Output: 
```
1
```

---

**Example 2**

Input: 
```
nums = [4, 5, 0, 1, 2, 3]
```

Output: 
```
0
```

---

**Example 3**

Input: 
```
nums = [4, 5, 6, 7]
```

Output: 
```
4
```

---

## Solution

### Java Code

```java
class Solution {
    public int findMin(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        int result = nums[0];

        while (left <= right) {
            // If the current window is already sorted, the leftmost element is the candidate
            if (nums[left] < nums[right]) {
                result = Math.min(result, nums[left]);
                break;
            }

            int mid = left + (right - left) / 2;
            result = Math.min(result, nums[mid]);

            // If mid is part of the left sorted portion, search the right portion
            if (nums[mid] >= nums[left]) {
                left = mid + 1;
            } else {
                // mid is part of the right sorted portion, search the left portion
                right = mid - 1;
            }
        }

        return result;
    }
}
```

---

## Java Utility Components Used

## 1. `Math.min()`

### What it is
A static utility method from the `java.lang.Math` class used to find the minimum of two numerical values.

### How it is used
```java
result = Math.min(result, nums[mid]);
```
We use it to continuously keep track of the minimum value encountered as we narrow down our search space using Binary Search.

---

## Intuition

Since the array is rotated but still consists of two sorted "halves", we can use **Binary Search** to find the pivot (the minimum element).

1. **Window Property**: If `nums[left] < nums[right]`, the current range is already sorted, so `nums[left]` is a minimum candidate.
2. **Binary Search**:
   - Compare `nums[mid]` with `nums[left]`.
   - If `nums[mid] >= nums[left]`, it means the left side is sorted and the rotation point (and thus the minimum) must be on the **right** side.
   - Otherwise, the rotation point is on the **left** side (including `mid`).
3. **Capture**: We update our `result` at each `mid` and when we find a sorted sub-segment.

---

## Complexity Analysis

### Time Complexity

```
O(log n)
```

Standard Binary Search performance as we halve the search space in each iteration.

---

### Space Complexity

```
O(1)
```

We only use a few integer variables (`left`, `right`, `mid`, `result`).

---

## Key Takeaways

- **Binary Search** isn't just for fully sorted arrays; it works on anything with a "step function" or searchable property.
- `Math.min()` keeps the comparison logic clean.
- Always handle the case where the current range becomes sorted during the search.
