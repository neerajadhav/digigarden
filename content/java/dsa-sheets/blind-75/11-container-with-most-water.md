---
title: 11 - Container With Most Water
tags:
  - Array
  - Two-Pointers
  - Medium
---

You are given an integer array `heights` where `heights[i]` represents the height of the $i^{th}$ bar.

You may choose any two bars to form a container. Return the **maximum amount of water** a container can store.

---

## Examples

**Example 1**

![[Pasted image 20260314134739.png]]

Input: 
```
height = [1, 7, 2, 5, 4, 7, 3, 6]
```

Output: 
```
36
```

---

**Example 2**

Input: 
```
height = [2, 2, 2]
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
    public int maxArea(int[] heights) {
        int maxArea = 0;
        int left = 0;
        int right = heights.length - 1;

        while (left < right) {
            // Calculate current area
            // Area = width * minHeight
            int width = right - left;
            int minHeight = Math.min(heights[left], heights[right]);
            int currentArea = width * minHeight;

            // Update maximum area
            maxArea = Math.max(maxArea, currentArea);

            // Move the pointer pointing to the shorter bar
            // Logic: Shorter bar limits the height, so we skip it to find a taller one
            if (heights[left] < heights[right]) {
                left++;
            } else {
                right--;
            }
        }

        return maxArea;
    }
}
```

---

## Java Collections Framework & Utility Components Used

## 1. `Math.min()` & `Math.max()`

### What they are
Static utility methods from the `java.lang.Math` class used to find the minimum and maximum of numerical values.

### How they are used
```java
int minHeight = Math.min(heights[left], heights[right]);
maxArea = Math.max(maxArea, currentArea);
```
These methods replace traditional `if-else` blocks for cleaner, more readable logic when comparing metrics like height and area.

---

## 2. Note on JCF
While this specific optimal solution uses primitive arrays for performance, in a real-world JCF-heavy context, `heights` could be passed as a `List<Integer>`, which would require using `list.get(index)` and `list.size()`.

---

## Intuition

The amount of water is determined by the **width** between two bars and the **height of the shorter bar**.
`Area = (RightIndex - LeftIndex) * min(Height[Left], Height[Right])`

1. **Start wide**: Place pointers at the leftmost and rightmost bars. This gives the maximum possible width.
2. **Move inward wisely**: To increase the area, we need a taller bar because the width will only decrease as we move pointers inward.
3. **The Greedy Choice**: Always move the pointer that points to the **shorter** bar. Moving the taller bar's pointer won't increase the area (it will still be limited by the shorter bar), but moving the shorter bar's pointer gives us a chance to find a taller one that might compensate for the reduced width.

---

## Complexity Analysis

### Time Complexity

```
O(n)
```

We traverse the array once using two pointers meeting in the middle.

---

### Space Complexity

```
O(1)
```

We only use a few integer variables (`left`, `right`, `maxArea`, etc.) regardless of input size.

---

## Key Takeaways

- **Two-pointer greedy approach** reduces the problem from $O(n^2)$ to $O(n)$.
- The **bottleneck** is always the shorter bar.
- Using `Math` utilities keeps the core logic concise.
