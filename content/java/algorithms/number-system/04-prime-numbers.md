---
title: 04 - Prime Numbers
tags:
  - Math
  - Number System
  - Primes
  - Factorization
---

# Prime Numbers

A prime number is a natural number greater than 1 that has no positive divisors other than 1 and itself.

## 1. Primality Testing
The goal is to determine if a given number $n$ is prime.

### Trial Division ($O(\sqrt{n})$)
We only need to check divisors up to $\sqrt{n}$ because if $n = a \times b$, then at least one factor must be $\le \sqrt{n}$.

```java
public static boolean isPrime(int n) {
    if (n <= 1) return false;
    if (n <= 3) return true;
    if (n % 2 == 0 || n % 3 == 0) return false;

    for (int i = 5; i * i <= n; i = i + 6) {
        if (n % i == 0 || n % (i + 2) == 0)
            return false;
    }
    return true;
}
```
*Note: The $6k \pm 1$ optimization significantly reduces the number of checks.*

## 2. Prime Factorization
Every integer $n > 1$ can be uniquely represented as a product of prime numbers.

### Trial Division ($O(\sqrt{n})$)
```java
import java.util.*;

public static List<Integer> getPrimeFactors(int n) {
    List<Integer> factors = new ArrayList<>();
    // Handle 2s
    while (n % 2 == 0) {
        factors.add(2);
        n /= 2;
    }
    // Handle odd numbers
    for (int i = 3; i * i <= n; i += 2) {
        while (n % i == 0) {
            factors.add(i);
            n /= i;
        }
    }
    // If n > 1, the remaining n is prime
    if (n > 1) factors.add(n);
    return factors;
}
```

## Fundamental Theorem of Arithmetic
Every integer greater than 1 either is a prime number itself or can be represented as the product of prime numbers and that, moreover, this representation is unique, up to (except for) the order of the factors.

## Sample Input and Output (n = 315)
### Output
```text
Is 315 prime? false
Prime Factors: 3, 3, 5, 7
```

## Complexity
| Metric | Complexity | Description |
| :--- | :--- | :--- |
| **Primality Test** | $O(\sqrt{n})$ | Checking divisors up to square root of $n$. |
| **Factorization** | $O(\sqrt{n})$ | Worst case for prime numbers or squares of large primes. |

## Applications
- Cryptography (RSA encryption depends on the difficulty of factoring large numbers).
- Generating unique identifiers.
- Number theory theorems (Fermat's Little Theorem, Euler's Totient Function).

## Key Takeaways
- **Efficiency**: Use $O(\sqrt{n})$ for individual checks; use Sieve for multiple checks.
- **Edge Cases**: Always handle $n \le 1$ explicitly.
- **Factorization**: The product of all prime factors will always equal the original number.
