---
title: 67 - Find Median From Data Stream
tags:
  - Heap
  - Design
  - Hard
---

The median is the middle value in a sorted list of integers. For lists of even length, there is no middle value, so the median is the mean of the two middle values.

Implement the `MedianFinder` class with:
- `addNum(int num)`: Adds an integer to the data stream.
- `findMedian()`: Returns the median of all elements so far.

---

## Examples

**Example 1**

Input:
`["MedianFinder", "addNum", 1, "findMedian", "addNum", 3, "findMedian", "addNum", 2, "findMedian"]`

Output:
`[null, null, 1.0, null, 2.0, null, 2.0]`

---

## Solution

### Java Code

```java
class MedianFinder {
    private PriorityQueue<Integer> small; // Max-heap: stores the smaller half
    private PriorityQueue<Integer> large; // Min-heap: stores the larger half

    public MedianFinder() {
        small = new PriorityQueue<>(Collections.reverseOrder());
        large = new PriorityQueue<>();
    }
    
    public void addNum(int num) {
        // 1. Add to the smaller half (max-heap)
        small.add(num);
        
        // 2. Balance: ensure small <= large (value-wise)
        // Move the largest element from 'small' to 'large'
        large.add(small.poll());
        
        // 3. Keep heaps balanced in size
        // If 'large' is bigger, move one back to 'small'
        // This ensures 'small' always has >= elements than 'large'
        if (small.size() < large.size()) {
            small.add(large.poll());
        }
    }
    
    public double findMedian() {
        if (small.size() > large.size()) {
            // Odd total elements: median is the top of the larger heap (small)
            return small.peek();
        } else {
            // Even total elements: average of roots
            return (small.peek() + large.peek()) / 2.0;
        }
    }
}
```

---

## Intuition

Calculating the median requires a sorted order. Sorting the entire list every time an element is added would be $O(N \log N)$ per addition. We can do better.

### Two Heaps Strategy
We can divide the data stream into two halves:
1.  **Lower Half**: Stored in a **Max-Heap** (`small`). The root is the largest of the small values.
2.  **Upper Half**: Stored in a **Min-Heap** (`large`). The root is the smallest of the large values.

By maintaining these two heaps such that their sizes differ by at most 1, we can find the median in $O(1)$ time:
- If total size is **odd**, the median is the top of the heap with more elements (we design it to be `small`).
- If total size is **even**, the median is the average of the tops of both heaps.

### Balancing Logic
Every time we add a number:
1.  Put it in the max-heap (`small`).
2.  Take the maximum element of `small` and move it to the min-heap (`large`). This ensures that the upper half actually contains larger numbers than the lower half.
3.  If `large` now has more elements than `small`, move the minimum element of `large` back to `small`.

---

## Complexity Analysis

### Time Complexity

- **addNum**: $O(\log n)$ due to the heap insertions and extractions.
- **findMedian**: $O(1)$ directly peeking at the roots.

### Space Complexity

- $O(n)$ to store all the numbers in the heaps.

---

## Key Takeaways

- The two-heap approach is the gold standard for dynamic median tracking.
- Max-heap stores small numbers; Min-heap stores large numbers.
- Always be careful with integer division when calculating the average for even lengths.
