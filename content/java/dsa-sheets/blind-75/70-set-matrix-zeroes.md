---
title: 70 - Set Matrix Zeroes
tags:
  - Matrix
  - Math & Geometry
  - Medium
---

Given an $m \times n$ matrix of integers `matrix`, if an element is $0$, set its entire row and column to $0$'s.

You must update the matrix **in-place**.

---

## Examples

**Example 1**

![Set Zeroes 1](../../../assets/attachments/set-zeroes-1.avif)

Input: 
```text
matrix = [
  [0,1],
  [1,0]
]
```
Output: 
```text
[
  [0,0],
  [0,0]
]
```

**Example 2**

![Set Zeroes 2](../../../assets/attachments/set-zeroes-2.avif)

Input: 
```text
matrix = [
  [1,2,3],
  [4,0,5],
  [6,7,8]
]
```
Output: 
```text
[
  [1,0,3],
  [0,0,0],
  [6,0,8]
]
```

---

## Solution

### Java Code

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;
        boolean firstRowZero = false;
        boolean firstColZero = false;

        // 1. Check if first row/column need to be zeroed later
        for (int i = 0; i < m; i++) {
            if (matrix[i][0] == 0) {
                firstColZero = true;
                break;
            }
        }
        for (int j = 0; j < n; j++) {
            if (matrix[0][j] == 0) {
                firstRowZero = true;
                break;
            }
        }

        // 2. Use first row and first column as markers
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }

        // 3. Set zeros based on markers in first row/column
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }

        // 4. Zero out the first row and first column if needed
        if (firstColZero) {
            for (int i = 0; i < m; i++) {
                matrix[i][0] = 0;
            }
        }
        if (firstRowZero) {
            for (int j = 0; j < n; j++) {
                matrix[0][j] = 0;
            }
        }
    }
}
```

---

## Intuition

A simple solution would be to use two sets or boolean arrays of size $M$ and $N$ to track which rows and columns should be zeroed. However, that costs $O(M + N)$ extra space. To achieve $O(1)$ space, we can use the matrix itself as store the markers.

### Using In-Place Markers
The first row and the first column of the matrix can serve as our "sets".
1.  **Detection**: If `matrix[i][j]` is 0, we mark its row (`matrix[i][0] = 0`) and its column (`matrix[0][j] = 0`).
2.  **Corner Case**: The very first element `matrix[0][0]` is shared by the first row and the first column. To avoid confusion, we handle the first row and first column's own zero status using two separate boolean variables (`firstRowZero`, `firstColZero`) and only use the rest of the first row/column for the others.
3.  **Application**: Iterate through the matrix (starting from `1,1`) and set elements to 0 if their corresponding row/column marker in the first row/column is 0.
4.  **Final Polish**: Finally, zero out the actual first row and first column based on our saved booleans.

---

## Complexity Analysis

### Time Complexity

$O(M \times N)$

- We traverse the matrix a few times (detection, application), but each traversal is linear relative to the total number of elements.

### Space Complexity

$O(1)$

- We only use a constant number of extra variables. All markers are stored within the input matrix.

---

## Key Takeaways

- To save space in matrix problems, look for ways to store state within the matrix itself (using headers or bit-manipulation).
- Always handle the crossing point (e.g., `matrix[0][0]`) carefully when using headers.
- Breaking the problem into clear phases (Detection -> Marking -> Applying) prevents overwriting data you still need.
