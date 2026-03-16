---
title: 63 - Meeting Rooms
tags:
  - Intervals
  - Array
  - Easy
---

Given an array of meeting time interval objects consisting of start and end times `[[start_1,end_1],[start_2,end_2],...]` (start_i < end_i), determine if a person could add all meetings to their schedule without any conflicts.

Note: `(0,8),(8,10)` is not considered a conflict at 8.

---

## Examples

**Example 1**

Input: `intervals = [(0,30),(5,10),(15,20)]`  
Output: `false`  
Explanation: `(0,30)` and `(5,10)` will conflict.

**Example 2**

Input: `intervals = [(5,8),(9,15)]`  
Output: `true`

---

## Solution

### Java Code

```java
/**
 * Definition of Interval:
 * public class Interval {
 *     public int start, end;
 *     public Interval(int start, int end) {
 *         this.start = start;
 *         this.end = end;
 *     }
 * }
 */

class Solution {
    public boolean canAttendMeetings(List<Interval> intervals) {
        if (intervals == null || intervals.size() <= 1) {
            return true;
        }

        // 1. Sort intervals by start time
        Collections.sort(intervals, (a, b) -> Integer.compare(a.start, b.start));

        // 2. Iterate through and check for overlaps
        for (int i = 0; i < intervals.size() - 1; i++) {
            // If the current meeting ends AFTER the next meeting starts
            if (intervals.get(i).end > intervals.get(i + 1).start) {
                return false;
            }
        }

        return true;
    }
}
```

---

## Intuition

The problem is essentially asking: "Are there any overlapping intervals in the given list?"

### Sorting-Based Check
If the intervals are processed in random order, we'd need to compare every pair ($O(n^2)$). However, if we **sort** the meetings by their **start times**, a collision can only happen between adjacent meetings in the sorted list.
- After sorting, for any two consecutive meetings $A$ and $B$, $A$ starts before or at the same time as $B$.
- A conflict exists if and only if $A.end > B.start$. (Note: $A.end = B.start$ is allowed).

---

## Complexity Analysis

### Time Complexity

$O(n \log n)$

- Sorting the list of $n$ intervals takes $O(n \log n)$.
- The subsequent single pass through the list takes $O(n)$.

### Space Complexity

$O(1)$ or $O(n)$

- $O(1)$ (or $O(\log n)$ for sorting stack) if we sort in-place.
- $O(n)$ if the sorting algorithm requires additional space or if we are not allowed to modify the input.

---

## Key Takeaways

- Sorting by start time is the standard "unlock" for most interval-related problems.
- Always clarify whether the endpoint is inclusive or exclusive ($>$ vs $\ge$).
- This is the "Easy" foundation for more complex problems like "Meeting Rooms II" (min rooms required) or "Merge Intervals".
