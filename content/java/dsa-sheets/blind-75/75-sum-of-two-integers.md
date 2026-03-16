---
title: 75 - Sum of Two Integers
tags:
  - Bit Manipulation
  - Medium
---

Given two integers `a` and `b`, return the sum of the two integers without using the `+` and `-` operators.

---

## Examples

**Example 1**

Input: `a = 1`, `b = 1`  
Output: `2`

**Example 2**

Input: `a = 4`, `b = 7`  
Output: `11`

---

## Solution

### Java Code

```java
class Solution {
    public int getSum(int a, int b) {
        // Continue until there is no carry left
        while (b != 0) {
            // Carry contains common set bits of a and b
            int carry = (a & b) << 1;
            
            // Sum of bits of a and b where at least one is not set
            a = a ^ b;
            
            // Update b with the carry
            b = carry;
        }
        return a;
    }
}
```

---

## Intuition

Adding two numbers manually involves two main steps for each bit:
1.  **Partial Sum**: Adding bits without considering the carry (similar to an XOR operation).
2.  **Carry**: Calculating the carry bits (similar to an AND operation, shifted left by 1).

### The Logic
*   **XOR (`a ^ b`)**: This gives us the "sum" bits where we don't have $1+1$. For example, $1 \oplus 0 = 1$, $0 \oplus 1 = 1$, but $1 \oplus 1 = 0$ (which is correct for the units place of $1+1=10_2$).
*   **AND + Shift (`(a & b) << 1`)**: This gives us the "carry" bits. $1 \& 1 = 1$, which means both bits were set, creating a carry for the **next** (left) position.
*   We repeat this process:
    1.  New `a` = Partial Sum.
    2.  New `b` = Carry.
3.  When the carry `b` becomes 0, the current `a` holds the final sum.

---

## Complexity Analysis

### Time Complexity

$O(1)$

- While it depends on the number of non-zero carry bits, for a 32-bit integer, the loop runs at most 32 times.

### Space Complexity

$O(1)$

- We only use one constant-space variable for the carry.

---

## Key Takeaways

- Binary addition is the foundation of ALU (Arithmetic Logic Unit) design.
- The XOR operator is often called "addition without carry".
- This approach works for negative numbers as well due to Java's **Two's Complement** representation of integers.
