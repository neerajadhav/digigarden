---
title: 69 - Spiral Matrix
tags:
  - Matrix
  - Math & Geometry
  - Medium
---

Given an $m \times n$ matrix of integers `matrix`, return a list of all elements within the matrix in spiral order.

---

## Examples

**Example 1**

![Spiral Matrix 1](../../../assets/attachments/spiral-matrix-1.avif)

Input: `matrix = [[1,2,3],[4,5,6],[7,8,9]]`  
Output: `[1,2,3,6,9,8,7,4,5]`

**Example 2**

![Spiral Matrix 2](../../../assets/attachments/spiral-matrix-2.avif)

Input: `matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]`  
Output: `[1,2,3,4,8,12,11,10,9,5,6,7]`

---

## Solution

### Java Code

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> result = new ArrayList<>();
        if (matrix == null || matrix.length == 0) {
            return result;
        }

        int top = 0;
        int bottom = matrix.length - 1;
        int left = 0;
        int right = matrix[0].length - 1;

        while (true) {
            // 1. Traverse Right
            for (int i = left; i <= right; i++) {
                result.add(matrix[top][i]);
            }
            if (++top > bottom) break;

            // 2. Traverse Down
            for (int i = top; i <= bottom; i++) {
                result.add(matrix[i][right]);
            }
            if (--right < left) break;

            // 3. Traverse Left
            for (int i = right; i >= left; i--) {
                result.add(matrix[bottom][i]);
            }
            if (--bottom < top) break;

            // 4. Traverse Up
            for (int i = bottom; i >= top; i--) {
                result.add(matrix[i][left]);
            }
            if (++left > right) break;
        }

        return result;
    }
}
```

---

## Intuition

Generating a spiral order is a matter of systematically traversing the perimeter of the matrix and shrinking the "active" boundaries inward.

### Boundary Simulation
Imagine we have 4 walls: `top`, `bottom`, `left`, and `right`.
1.  **Top Row**: Move from `left` to `right` along the `top` row. Once done, the `top` row is finished, so increment `top`.
2.  **Right Column**: Move from `top` to `bottom` along the `right` column. Once done, decrement `right`.
3.  **Bottom Row**: Move from `right` to `left` along the `bottom` row. Once done, decrement `bottom`.
4.  **Left Column**: Move from `bottom` to `top` along the `left` column. Once done, increment `left`.

We repeat this cycle until our boundaries cross (`top > bottom` or `left > right`), indicating that we've visited every possible cell.

---

## Complexity Analysis

### Time Complexity

$O(M \cdot N)$

- We visit every element in the $M \times N$ matrix exactly once.

### Space Complexity

$O(1)$

- We ignore the output list space. The simulation uses a handful of integer variables for boundaries.

---

## Key Takeaways

- Boundary simulation is the cleanest way to handle spiral/layer traversal.
- Always check the exit condition immediately after shrinking a boundary.
- Be careful with the `while` loop condition to avoid redundant checks at the end of a partial spiral.
