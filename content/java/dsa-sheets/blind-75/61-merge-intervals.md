---
title: 61 - Merge Intervals
tags:
  - Intervals
  - Array
  - Medium
---

Given an array of intervals where `intervals[i] = [start_i, end_i]`, merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

---

## Examples

**Example 1**

Input: `intervals = [[1,3],[1,5],[6,7]]`  
Output: `[[1,5],[6,7]]`  
Explanation: Intervals `[1,3]` and `[1,5]` overlap, so they are merged into `[1,5]`.

**Example 2**

Input: `intervals = [[1,2],[2,3]]`  
Output: `[[1,3]]`

---

## Solution

### Java Code

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        if (intervals.length <= 1) {
            return intervals;
        }

        // 1. Sort intervals by start time
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));

        List<int[]> result = new ArrayList<>();
        
        // 2. Initialize with the first interval
        int[] currentInterval = intervals[0];
        result.add(currentInterval);

        for (int[] interval : intervals) {
            int currentEnd = currentInterval[1];
            int nextStart = interval[0];
            int nextEnd = interval[1];

            // 3. Check for overlap
            if (currentEnd >= nextStart) {
                // Update the end of the current interval to the max possible
                currentInterval[1] = Math.max(currentEnd, nextEnd);
            } else {
                // No overlap, add the next interval as the new 'current'
                currentInterval = interval;
                result.add(currentInterval);
            }
        }

        return result.toArray(new int[result.size()][]);
    }
}
```

---

## Intuition

The core problem is to find and combine intervals that overlap. If we process intervals in a random order, we'd have to compare every interval with every other interval ($O(n^2)$).

### Sorting + Single Pass
By **sorting** the intervals based on their **start times**, we ensure that overlapping intervals are placed adjacent to each other.
- Once sorted, we only need to compare the current interval we are "building" with the next interval in the list.
- If the current interval's `end` is greater than or equal to the next interval's `start`, they overlap. We merge them by setting the `end` to the maximum of both.
- If they don't overlap, we are done with the current one and move on to the next one.

---

## Complexity Analysis

### Time Complexity

$O(n \log n)$

- Sorting the array costs $O(n \log n)$.
- The merging process involves a single pass through the sorted array, costing $O(n)$.

### Space Complexity

$O(\log n)$ or $O(n)$

- $O(\log n)$ is typically required for the internal sorting algorithm.
- $O(n)$ if we count the space used to store the merged output.

---

## Key Takeaways

- Sorting is often the most critical first step for interval problems.
- Always check if the comparison should be strictly greater or greater-than-or-equal ($>=$ vs $>$).
- In Java, using `Arrays.sort` with a lambda comparator is convenient, but be mindful of overflow if using subtraction (e.g., `a[0] - b[0]`); `Integer.compare` is safer.
