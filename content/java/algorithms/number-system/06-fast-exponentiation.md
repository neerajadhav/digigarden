---
title: 06 - Fast Exponentiation
tags:
  - Math
  - Number System
  - Binary Exponentiation
---

# Fast Exponentiation

Fast Exponentiation, also known as **Binary Exponentiation**, is a technique to calculate $a^n$ in $O(\log n)$ time instead of the naive $O(n)$.

## Intuition
The idea is to divide the power by half in each step.
- If $n$ is even: $a^n = (a^2)^{n/2}$
- If $n$ is odd: $a^n = a \times a^{n-1} = a \times (a^2)^{(n-1)/2}$

## 1. Recursive Implementation

```java
public static long powerRecursive(long a, long n) {
    if (n == 0) return 1;
    long res = powerRecursive(a, n / 2);
    if (n % 2 == 0) {
        return res * res;
    } else {
        return res * res * a;
    }
}
```

## 2. Iterative Implementation
This is more efficient as it avoids the recursive stack overhead. It uses the binary representation of $n$.

```java
public static long powerIterative(long a, long n) {
    long res = 1;
    while (n > 0) {
        if ((n & 1) == 1) { // If n is odd
            res = res * a;
        }
        a = a * a;
        n >>= 1; // Divide n by 2
    }
    return res;
}
```

## 3. Modular Exponentiation
In competitive programming, we often need $(a^n) \pmod m$. This is crucial to prevent overflow.

```java
public static long powerModular(long a, long n, long m) {
    long res = 1;
    a %= m;
    while (n > 0) {
        if ((n & 1) == 1) {
            res = (res * a) % m;
        }
        a = (a * a) % m;
        n >>= 1;
    }
    return res;
}
```

## Complexity
| Metric | Complexity | Description |
| :--- | :--- | :--- |
| **Time** | $O(\log n)$ | Each step reduces $n$ by half. |
| **Space** | $O(1)$ | Iterative version uses constant extra space. |

## Sample Input and Output
### Input
```text
base = 3, exp = 10
```
### Output
```text
3^10 = 59049
```

## Applications
- Calculating large powers efficiently.
- Modular inverse (using Fermat’s Little Theorem).
- Matrix Exponentiation (e.g., for finding $N$-th Fibonacci number in $O(\log n)$).
- Cryptography (RSA algorithm).

## Key Takeaways
- **Efficiency**: Reduces computations from billions to less than 64 for a 64-bit exponent.
- **Modulo**: Always perform modulo at each multiplication step to avoid overflow.
- **Negative Exponent**: For $a^{-n}$, calculate $1 / a^n$.
