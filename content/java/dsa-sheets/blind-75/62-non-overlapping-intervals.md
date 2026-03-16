---
title: 62 - Non-overlapping Intervals
tags:
  - Intervals
  - Greedy
  - Medium
---

Given an array of intervals `intervals` where `intervals[i] = [start_i, end_i]`, return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

Note: Intervals are non-overlapping even if they have a common point (e.g., `[1, 2]` and `[2, 3]` are non-overlapping).

---

## Examples

**Example 1**

Input: `intervals = [[1,2],[2,4],[1,4]]`  
Output: `1`  
Explanation: After `[1,4]` is removed, the rest of the intervals are non-overlapping.

**Example 2**

Input: `intervals = [[1,2],[2,4]]`  
Output: `0`

---

## Solution

### Java Code

```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        if (intervals.length == 0) {
            return 0;
        }

        // 1. Sort intervals by their END times
        // This is a classic greedy interval scheduling approach.
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[1], b[1]));

        int count = 0;
        int lastEnd = intervals[0][1];

        for (int i = 1; i < intervals.length; i++) {
            // 2. Check for overlap: current interval starts before the previous one ends
            if (intervals[i][0] < lastEnd) {
                // Must remove one interval to resolve overlap. 
                // Because we sorted by end time, we greedily keep the one that ends earlier.
                count++;
            } else {
                // No overlap: update the end time to the current interval's end
                lastEnd = intervals[i][1];
            }
        }

        return count;
    }
}
```

---

## Intuition

This problem is a variation of the **Interval Scheduling Problem**. To minimize the number of removals, we want to maximize the number of non-overlapping intervals we can keep.

### Greedy Choice
The optimal greedy strategy is to always pick the interval that **finishes earliest**.
- Why? Because an interval that finishes earlier leaves the maximum amount of "room" for remaining intervals to fit.
- If we sort by start time, we might pick a very long interval that blocks many others.
- By sorting by **end time**, we ensure that we are making the most "efficient" use of our timeline.

Whenever we encounter an overlap (i.e., the current interval starts before the last kept interval ends), we increment our removal count. Otherwise, we "keep" the current interval by updating our `lastEnd` marker.

---

## Complexity Analysis

### Time Complexity

$O(n \log n)$

- Sorting the intervals takes $O(n \log n)$.
- The subsequent linear scan takes $O(n)$.

### Space Complexity

$O(1)$ or $O(n)$

- $O(1)$ (or $O(\log n)$ for sorting stack) if we sort in-place.
- $O(n)$ if the sorting algorithm requires additional space or if we count the result list.

---

## Key Takeaways

- To **maximize** non-overlapping intervals, we greedily select based on **endpoint**.
- The number of removals is simply `Total Intervals - Max Non-Overlapping Intervals`.
- Overlap condition: `next.start < current.end`. Note that "non-overlapping even if they have a common point" means we use `<` instead of `<=`.
