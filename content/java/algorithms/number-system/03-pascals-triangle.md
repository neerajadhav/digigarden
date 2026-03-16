---
title: 03 - Pascal's Triangle
tags:
  - Math
  - Number System
  - Binomial Coefficient
---

# Pascal's Triangle

Pascal's Triangle is a triangular array of binomial coefficients. Each number in the triangle is the sum of the two numbers directly above it.

## Visualization
```text
    1
   1 1
  1 2 1
 1 3 3 1
1 4 6 4 1
```

## Mathematical Properties
1.  **Binomial Expansion**: The $n$-th row of Pascal's triangle represents the coefficients of the expansion of $(x + y)^n$.
2.  **Binomial Coefficient**: The element at row $n$, column $k$ is given by $C(n, k) = \frac{n!}{k!(n-k)!}$.
3.  **Sum of Rows**: The sum of elements in the $n$-th row is $2^n$.
4.  **Symmetry**: The triangle is symmetric, $C(n, k) = C(n, n-k)$.

## 1. Standard Implementation ($O(n^2)$ Space)
Using a 2D array to store the entire triangle.

```java
import java.util.*;

public class PascalsTriangle {
    public static List<List<Integer>> generate(int numRows) {
        List<List<Integer>> triangle = new ArrayList<>();
        if (numRows == 0) return triangle;

        for (int i = 0; i < numRows; i++) {
            List<Integer> row = new ArrayList<>();
            for (int j = 0; j <= i; j++) {
                if (j == 0 || j == i) {
                    row.add(1);
                } else {
                    int val = triangle.get(i - 1).get(j - 1) + triangle.get(i - 1).get(j);
                    row.add(val);
                }
            }
            triangle.add(row);
        }
        return triangle;
    }
}
```

## 2. Space Optimized Implementation ($O(n)$ Space)
If we only need a specific row, we can calculate it in $O(n)$ space.

```java
public static List<Integer> getRow(int rowIndex) {
    List<Integer> row = new ArrayList<>();
    long val = 1;
    for (int i = 0; i <= rowIndex; i++) {
        row.add((int) val);
        val = val * (rowIndex - i) / (i + 1);
    }
    return row;
}
```
*Note: Using `long` for `val` to prevent overflow during intermediate calculations.*

## Complexity
| Metric | Complexity | Description |
| :--- | :--- | :--- |
| **Time** | $O(n^2)$ | To generate $n$ rows, we process $1 + 2 + ... + n$ elements. |
| **Space** | $O(n^2)$ | Standard implementation stores the entire triangle. |
| **Space (Optimized)**| $O(n)$ | If generating only one row or using 1D DP. |

## Sample Input and Output (numRows = 5)
### Output
```text
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

## Applications
- Combinatorics and Probability.
- Finding Binomial Coefficients ($nCr$).
- Polynomial expansions.
- Calculating path combinations in a grid.

## Key Takeaways
- **Formula**: `row[i][j] = row[i-1][j-1] + row[i-1][j]`.
- **Optimization**: You can generate the $k$-th row in $O(k)$ time and $O(1)$ auxiliary space if you use the multiplicative formula.
- **Overflow**: Always be cautious with large row numbers as binomial coefficients grow exponentially.
