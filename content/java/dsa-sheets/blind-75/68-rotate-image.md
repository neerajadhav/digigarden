---
title: 68 - Rotate Image
tags:
  - Matrix
  - Math & Geometry
  - Medium
---

Given a square $n \times n$ matrix of integers `matrix`, rotate it by 90 degrees clockwise.

You must rotate the matrix **in-place**. Do not allocate another 2D matrix.

---

## Examples

**Example 1**

![Rotate Image 1](../../../assets/attachments/rotate-image-1.avif)

Input: 
```text
matrix = [
  [1,2],
  [3,4]
]
```
Output: 
```text
[
  [3,1],
  [4,2]
]
```

**Example 2**

![Rotate Image 2](../../../assets/attachments/rotate-image-2.avif)

Input: 
```text
matrix = [
  [1,2,3],
  [4,5,6],
  [7,8,9]
]
```
Output: 
```text
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```

---

## Solution

### Java Code

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;

        // 1. Transpose the matrix
        // (Swap matrix[i][j] with matrix[j][i])
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }

        // 2. Reflect the matrix horizontally
        // (Reverse each row)
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n / 2; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[i][n - 1 - j];
                matrix[i][n - 1 - j] = temp;
            }
        }
    }
}
```

---

## Intuition

Rotating a matrix clockwise by 90 degrees can be achieved through two simpler geometric transformations:

1.  **Transpose**: Converting rows to columns. After transposing, the element at `(i, j)` moves to `(j, i)`.
2.  **Reflect**: Flipping the matrix horizontally. The first column becomes the last, the second becomes the second-to-last, and so on.

### why this works?
If you transpose a matrix, you've essentially rotated it 90 degrees and flipped it over the diagonal. Reversing each row "corrects" this flip, resulting in a perfect 90-degree clockwise rotation.

---

## Complexity Analysis

### Time Complexity

$O(N^2)$

- Transposing involves visiting roughly half the elements in the $N \times N$ grid, which is $O(N^2)$.
- Reflecting (reversing rows) also takes $O(N^2)$.

### Space Complexity

$O(1)$

- The rotation is performed strictly in-place, using only a tiny amount of stack space for variables.

---

## Key Takeaways

- Transpose + Reflect is the most intuitive and clean way to perform matrix rotation.
- For a **counter-clockwise** rotation, you would transpose and then reflect **vertically** (reverse the order of rows).
- Always ensure you only transpose the upper or lower triangle of the matrix; otherwise, you'll swap elements twice and end up with the original matrix!
