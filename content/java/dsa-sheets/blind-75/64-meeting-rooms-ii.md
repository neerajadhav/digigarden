---
title: 64 - Meeting Rooms II
tags:
  - Intervals
  - Greedy
  - Medium
---

Given an array of meeting time interval objects consisting of start and end times `[[start_1,end_1],[start_2,end_2],...]` (start_i < end_i), find the minimum number of rooms required to schedule all meetings without any conflicts.

Note: `(0,8),(8,10)` is NOT considered a conflict at 8.

---

## Examples

**Example 1**

Input: `intervals = [(0,40),(5,10),(15,20)]`  
Output: `2`  
Explanation: 
- Room 1: `(0,40)`
- Room 2: `(5,10)`, `(15,20)`

**Example 2**

Input: `intervals = [(4,9)]`  
Output: `1`

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
    public int minMeetingRooms(List<Interval> intervals) {
        if (intervals == null || intervals.isEmpty()) {
            return 0;
        }

        int n = intervals.size();
        int[] starts = new int[n];
        int[] ends = new int[n];

        // 1. Separate starts and ends into two separate arrays
        for (int i = 0; i < n; i++) {
            starts[i] = intervals.get(i).start;
            ends[i] = intervals.get(i).end;
        }

        // 2. Sort both arrays
        Arrays.sort(starts);
        Arrays.sort(ends);

        int rooms = 0;
        int endPtr = 0;

        // 3. Use two pointers to simulate the timeline
        for (int startPtr = 0; startPtr < n; startPtr++) {
            // If the earliest-starting meeting begins BEFORE the earliest-ending meeting finishes
            if (starts[startPtr] < ends[endPtr]) {
                // We need a NEW room
                rooms++;
            } else {
                // A meeting has finished, we can reuse its room
                // Move endPtr to the next meeting's end time
                endPtr++;
            }
        }

        return rooms;
    }
}
```

---

## Intuition

The problem asks for the maximum number of **simultaneous** meetings happening at any point in time.

### Separate Timeline Strategy
We don't actually care *which* specific meeting is in which room. We only care about how many meetings have started and how many have ended at any given timestamp.

1.  **Extract and Sort**: Put all start times in one array and all end times in another. Sort both.
2.  **Two Pointers**:
    - Iterate through the `starts` array. Each `starts[i]` represents a meeting beginning.
    - Compare `starts[startPtr]` with `ends[endPtr]`.
    - If a meeting starts before the earliest meeting ends (`starts < ends`), it means all currently occupied rooms are still busy. Increment `rooms`.
    - If a meeting starts after or at the same time as a meeting ends (`starts >= ends`), a room has become vacant. We don't need a new room; we just move `endPtr` to account for the vacancy.

This greedy approach effectively measures the "peak occupancy" of the meeting rooms.

---

## Complexity Analysis

### Time Complexity

$O(n \log n)$

- Extracting starts and ends is $O(n)$.
- Sorting both arrays is $O(n \log n)$.
- The two-pointer traversal is $O(n)$.

### Space Complexity

$O(n)$

- We store the start and end times in separate arrays of size $n$.

---

## Key Takeaways

- Dividing start and end events is a powerful technique for "maximum concurrent events" problems.
- Note the comparison logic: `starts[startPtr] < ends[endPtr]`. If it were `<=`, a meeting starting exactly when another ends would require a new room. Since the problem states `(0,8),(8,10)` is not a conflict, we use `<`.
- Alternative approach: Use a **Min-Heap (PriorityQueue)** to track the end times of currently active meetings. The heap size at the end will be the answer.
