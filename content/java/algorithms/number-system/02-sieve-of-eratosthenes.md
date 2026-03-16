---
title: 02 - Sieve of Eratosthenes
tags:
  - Math
  - Number System
  - Primes
---

# Sieve of Eratosthenes

The Sieve of Eratosthenes is an ancient and highly efficient algorithm for finding all prime numbers up to a specified integer $n$.

## Intuition
Instead of checking each number for primality (which is $O(n \sqrt{n})$), we start from the first prime (2) and mark all its multiples as composite. We then move to the next unmarked number and repeat.

## Algorithm Steps
1.  **Initialize**: Create a boolean array `isPrime` of size $n+1$ and set all entries to `true`. Set `isPrime[0]` and `isPrime[1]` to `false`.
2.  **Iterate**: For $p = 2$ to $\sqrt{n}$:
    - If `isPrime[p]` is `true`:
      - **Mark**: Mark all multiples of $p$ starting from $p^2$ as `false`.
3.  **Collect**: All indices with `true` values are prime.

## Java Implementation

```java
import java.util.*;

public class Sieve {
    public static void sieve(int n) {
        boolean[] isPrime = new boolean[n + 1];
        Arrays.fill(isPrime, true);
        isPrime[0] = isPrime[1] = false;

        for (int p = 2; p * p <= n; p++) {
            // If isPrime[p] is not changed, then it is a prime
            if (isPrime[p]) {
                // Update all multiples of p starting from p*p
                for (int i = p * p; i <= n; i += p) {
                    isPrime[i] = false;
                }
            }
        }

        // Print all prime numbers
        System.out.println("Primes up to " + n + ":");
        for (int i = 2; i <= n; i++) {
            if (isPrime[i]) {
                System.out.print(i + " ");
            }
        }
        System.out.println();
    }

    public static void main(String[] args) {
        sieve(30);
    }
}
```

## Optimizations
1.  **Inner Loop Start**: We start the inner loop from $j = i \times i$. This is because any multiple $j = i \times k$ where $k < i$ would have already been marked by some prime factor of $k$.
2.  **Outer Loop Limit**: We only need to iterate up to $\sqrt{n}$ because every composite number $c \le n$ must have a prime factor $\le \sqrt{c} \le \sqrt{n}$.

## Sample Input and Output

### Input
```text
30
```

### Output
```text
Primes up to 30:
2 3 5 7 11 13 17 19 23 29 
```

## Complexity
| Metric | Complexity | Description |
| :--- | :--- | :--- |
| **Time** | $O(n \log \log n)$ | Sum of reciprocals of primes up to $n$ behaves as $\log \log n$. |
| **Space** | $O(n)$ | Requires a boolean array of size $n+1$. |

## Applications
- Precalculating primes for competitive programming problems.
- Finding prime factors of multiple numbers efficiently.
- Segmented Sieve for finding primes in a specific range.

## Key Takeaways
- **Efficiency**: Significantly faster than checking primality for each number individually.
- **Initialization**: Always remember to handle 0 and 1 correctly.
- **Space Limitation**: For extremely large $n$ (above $10^7$ or $10^8$), memory might become an issue.
