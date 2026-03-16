---
title: 01 - Fibonacci Sequence
tags:
  - Recursion
  - Dynamic Programming
  - Math
---

# Fibonacci Sequence

The Fibonacci sequence is a classic series where each number is the sum of the two preceding ones, usually starting with 0 and 1.
$$F(n) = F(n-1) + F(n-2)$$

## 1. Standard Recursion
The most straightforward implementation, but highly inefficient for large $n$ due to redundant calculations.

### Java Implementation
```java
public static long fib(int n) {
    if (n <= 1) return n;
    return fib(n - 1) + fib(n - 2);
}
```
**Complexity**: Time $O(2^n)$, Space $O(n)$.

## 2. Memoization (Top-Down DP)
Optimizes recursion by storing the results of sub-problems in an array or map.

### Java Implementation
```java
public static long fibMemo(int n, long[] memo) {
    if (n <= 1) return n;
    if (memo[n] != 0) return memo[n];
    return memo[n] = fibMemo(n - 1, memo) + fibMemo(n - 2, memo);
}
```
**Complexity**: Time $O(n)$, Space $O(n)$.

## 3. Tabulation & Space Optimization
Bottom-up approach using a loop. We only need the last two values to calculate the next one, allowing for $O(1)$ space.

### Java Implementation
```java
public static long fibOptimized(int n) {
    if (n <= 1) return n;
    long prev2 = 0, prev1 = 1;
    for (int i = 2; i <= n; i++) {
        long curr = prev1 + prev2;
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```
**Complexity**: Time $O(n)$, Space $O(1)$.

## 4. Matrix Exponentiation ($O(\log n)$)
By expressing Fibonacci as a matrix transformation, we can use binary exponentiation to find $F(n)$ in logarithmic time.
$$\begin{pmatrix} F(n+1) \\ F(n) \end{pmatrix} = \begin{pmatrix} 1 & 1 \\ 1 & 0 \end{pmatrix}^n \begin{pmatrix} F(1) \\ F(0) \end{pmatrix}$$

### Java Implementation
```java
public static long[][] multiply(long[][] A, long[][] B) {
    long[][] C = new long[2][2];
    for (int i = 0; i < 2; i++)
        for (int j = 0; j < 2; j++)
            for (int k = 0; k < 2; k++)
                C[i][j] += A[i][k] * B[k][j];
    return C;
}

public static long[][] power(long[][] A, int p) {
    long[][] res = {{1, 0}, {0, 1}};
    while (p > 0) {
        if (p % 2 == 1) res = multiply(res, A);
        A = multiply(A, A);
        p /= 2;
    }
    return res;
}

public static long fibMatrix(int n) {
    if (n <= 1) return n;
    long[][] T = {{1, 1}, {1, 0}};
    T = power(T, n - 1);
    return T[0][0];
}
```

## Java Code Template (Competitive Format)

```java
import java.util.*;

public class Main {
    // ... insert optimized implementations here ...

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        if (sc.hasNextInt()) {
            int t = sc.nextInt();
            while (t-- > 0) {
                int n = sc.nextInt();
                System.out.println("Fibonacci(" + n + ") = " + fibOptimized(n));
            }
        }
        sc.close();
    }
}
```

## Sample Input and Output

### Input
```text
2
10
50
```

### Output
```text
Fibonacci(10) = 55
Fibonacci(50) = 12586269025
```

## Complexity Comparison
| Approach | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **Recursion** | $O(2^n)$ | $O(n)$ |
| **Memoization** | $O(n)$ | $O(n)$ |
| **Tabulation** | $O(n)$ | $O(1)$ |
| **Matrix Exp** | $O(\log n)$ | $O(1)$ |

## Key Takeaways
- **Recursion** results in an exponential number of calls due to overlapping sub-problems.
- **Dynamic Programming** reduces the complexity to linear.
- **Matrix Exponentiation** is the gold standard for extremely large $n$.
