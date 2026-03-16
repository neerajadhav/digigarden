---
title: 01 - Greatest Common Divisor (GCD)
tags:
  - Math
  - Number System
  - Euclidean Algorithm
---

# Greatest Common Divisor (GCD)

The Greatest Common Divisor (GCD) of two or more integers is the largest positive integer that divides each of the integers without a remainder.

## Euclidean Algorithm
The Euclidean algorithm is an efficient method for computing the GCD of two numbers. It is based on the principle that the GCD of two numbers does not change if the larger number is replaced by its difference with the smaller number.

In a more efficient modular version:
$\text{gcd}(a, b) = \text{gcd}(b, a \mod b)$

## 1. Recursive Implementation
This follows the mathematical definition directly.

```java
public static int gcdRecursive(int a, int b) {
    if (b == 0) return a;
    return gcdRecursive(b, a % b);
}
```

## 2. Iterative Implementation
Often preferred to avoid stack overflow for very large computations, although for GCD, the recursive depth is very small ($O(\log(\min(a, b)))$).

```java
public static int gcdIterative(int a, int b) {
    while (b != 0) {
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}
```

## Mathematical Properties
- **$\text{gcd}(a, b) = \text{gcd}(b, a)$**
- **$\text{gcd}(a, 0) = a$**
- **$\text{gcd}(a, 1) = 1$**
- **LCM Relationship**: $\text{lcm}(a, b) = \frac{|a \times b|}{\text{gcd}(a, b)}$

## Sample Input and Output

### Input
```text
48 18
```

### Output
```text
GCD of 48 and 18 is 6
```

## Complexity
| Metric | Complexity | Description |
| :--- | :--- | :--- |
| **Time** | $O(\log(\min(a, b)))$ | Number of steps follows Lamé's theorem, related to Fibonacci numbers. |
| **Space** | $O(1)$ | Iterative approach uses constant space. Recursive uses $O(\log n)$ stack space. |

## Applications
- Simplifying fractions.
- Fundamental step in RSA cryptography.
- Finding Least Common Multiple (LCM).
- Solving Linear Diophantine Equations.
