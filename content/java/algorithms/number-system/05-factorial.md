---
title: 05 - Factorial
tags:
  - Math
  - Number System
  - Recursion
---

# Factorial

The factorial of a non-negative integer $n$, denoted by $n!$, is the product of all positive integers less than or equal to $n$.
$n! = n \times (n-1) \times (n-2) \times ... \times 1$

## 1. Iterative Implementation
Efficient and avoids recursion overhead.

```java
public static long factorialIterative(int n) {
    long res = 1;
    for (int i = 2; i <= n; i++) {
        res *= i;
    }
    return res;
}
```

## 2. Recursive Implementation
A classic example of recursion.
$n! = n \times (n-1)!$ base case: $0! = 1$.

```java
public static long factorialRecursive(int n) {
    if (n == 0) return 1;
    return n * factorialRecursive(n - 1);
}
```

## 3. BigInteger for Large Factorials
`long` in Java can only store up to $20!$. For larger numbers, use `java.math.BigInteger`.

```java
import java.math.BigInteger;

public static BigInteger factorialLarge(int n) {
    BigInteger res = BigInteger.ONE;
    for (int i = 2; i <= n; i++) {
        res = res.multiply(BigInteger.valueOf(i));
    }
    return res;
}
```

## Trailing Zeros in Factorial
The number of trailing zeros in $n!$ is determined by the count of factors of 5 in the prime factorization of $x!$.
Formula: $\lfloor n/5 \rfloor + \lfloor n/25 \rfloor + \lfloor n/125 \rfloor + ...$

```java
public static int countTrailingZeros(int n) {
    int count = 0;
    while (n >= 5) {
        n /= 5;
        count += n;
    }
    return count;
}
```

## Complexity
| Metric | Complexity | Description |
| :--- | :--- | :--- |
| **Time** | $O(n)$ | One loop or $n$ recursive calls. |
| **Space** | $O(1)$ | Iterative uses constant space. Recursive uses $O(n)$ stack space. |

## Applications
- Permutations: $P(n, r) = \frac{n!}{(n-r)!}$
- Combinations: $C(n, r) = \frac{n!}{r!(n-r)!}$
- Probability theory.

## Key Takeaways
- **Overflow**: $n!$ grows extremely fast. $21!$ overflows a 64-bit `long`.
- **Base Case**: Always define $0! = 1$ to ensure correct recursive results.
- **Zeros**: Counting trailing zeros is a common competitive programming trick using power of 5.
