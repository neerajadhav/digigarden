---
title: 60 - Insert Interval
tags:
  - Intervals
  - Array
  - Medium
---

You are given an array of non-overlapping intervals `intervals` sorted in ascending order by start time. You are also given another interval `newInterval`.

Insert `newInterval` into `intervals` such that the result is still sorted and contains no overlapping intervals. You may merge overlapping intervals if necessary.

---

## Examples

**Example 1**

Input: `intervals = [[1,3],[4,6]]`, `newInterval = [2,5]`  
Output: `[[1,6]]`

**Example 2**

Input: `intervals = [[1,2],[3,5],[9,10]]`, `newInterval = [6,7]`  
Output: `[[1,2],[3,5],[6,7],[9,10]]`

---

## Solution

### Java Code

```java
class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        List<int[]> result = new ArrayList<>();
        int i = 0;
        int n = intervals.length;

        // 1. Add all intervals that end before the new interval starts
        while (i < n && intervals[i][1] < newInterval[0]) {
            result.add(intervals[i]);
            i++;
        }

        // 2. Merge all overlapping intervals with the new interval
        while (i < n && intervals[i][0] <= newInterval[1]) {
            newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
            newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
            i++;
        }
        result.add(newInterval);

        // 3. Add all remaining intervals
        while (i < n) {
            result.add(intervals[i]);
            i++;
        }

        return result.toArray(new int[result.size()][]);
    }
}
```

---

## Intuition

The goal is to insert a new interval into an already sorted list of non-overlapping intervals while maintaining the sorting and non-overlapping properties.

### Three-Phase Approach
1.  **Before Intersection**: Iterate through the sorted list and add any interval that completely ends before the `newInterval` begins.
2.  **During Intersection**: While the current interval overlaps with the `newInterval` (meaning the interval starts before or at the end of the new one), merge them by updating the `newInterval`'s boundaries.
3.  **After Intersection**: Add the merged `newInterval` and then append all remaining intervals from the original list.

---

## Complexity Analysis

### Time Complexity

$O(n)$

- We traverse the input list exactly once using a single index `i`.

### Space Complexity

$O(n)$

- We use a list to store the result, which in the worst case contains all original intervals plus one (if nothing was merged).

---

## Key Takeaways

- Since the input is already sorted, we can solve this in a single pass without needing to re-sort.
- The condition for overlap between two intervals $[a, b]$ and $[c, d]$ is $\max(a, c) \le \min(b, d)$.
- Always consider the edge case where the input list is empty or the new interval goes at the very beginning or end.
