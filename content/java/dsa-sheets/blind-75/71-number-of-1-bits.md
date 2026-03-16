---
title: 71 - Number of 1 Bits
tags:
  - Bit Manipulation
  - Easy
---

You are given an unsigned integer `n`. Return the number of `1` bits in its binary representation (also known as the Hamming weight).

You may assume `n` is a non-negative integer which fits within 32-bits.

---

## Examples

**Example 1**

Input: `n = 00000000000000000000000000010111`  
Output: `4`  
Explanation: The input binary string has four `1` bits.

**Example 2**

Input: `n = 01111111111111111111111111111101`  
Output: `30`

---

## Solution

### Java Code

```java
class Solution {
    public int hammingWeight(int n) {
        int count = 0;
        
        // Use Brian Kernighan's Algorithm
        // n & (n - 1) always clears the rightmost set bit
        while (n != 0) {
            n = n & (n - 1);
            count++;
        }
        
        return count;
    }
}
```

---

## Intuition

The common way to count bits is to iterate through all 32 bits and check each one (`n & 1`). However, we can optimize this to only iterate over the bits that are actually set to `1`.

### Brian Kernighan's Algorithm
The expression `n & (n - 1)` has a very specific property: it **clears the least significant (rightmost) set bit** in `n`.
- For example, if $n = 12$ (`1100`), then $n - 1 = 11$ (`1011`).
- $1100 \& 1011 = 1000$ (`8`). The rightmost 1 at position 2 is gone.
- Repeating this operation until $n$ becomes 0 will give us exactly the count of set bits.

---

## Complexity Analysis

### Time Complexity

$O(k)$

- Where $k$ is the number of set bits (1s) in the integer.
- In the worst case (all bits set), it takes $O(32) \approx O(1)$.

### Space Complexity

$O(1)$

- We only use a single integer variable to keep track of the count.

---

## Key Takeaways

- Brian Kernighan's Algorithm is the most efficient way to count bits manually.
- In a real Java interview, you could also mention `Integer.bitCount(n)`, which uses a highly optimized bit-masking technique (parallel bit counting).
- Understanding `n & (n - 1)` is fundamental for many bit manipulation tricks (like checking if a number is a power of 2).
