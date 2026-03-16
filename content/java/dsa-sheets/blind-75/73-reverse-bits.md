---
title: 73 - Reverse Bits
tags:
  - Bit Manipulation
  - Easy
---

Given a 32-bit unsigned integer `n`, reverse the bits of the binary representation of `n` and return the result.

Note: In some languages such as Java, there is no unsigned integer type. In this case, both input and output will be given as a signed integer type. They should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.

---

## Example

Input: `n = 00000000000000000000000000010101`  
Output: `2818572288 (10101000000000000000000000000000)`

Explanation: Reversing `00000000000000000000000000010101` (21) gives `10101000000000000000000000000000` (2818572288).

---

## Solution

### Java Code

```java
class Solution {
    public int reverseBits(int n) {
        int res = 0;
        
        for (int i = 0; i < 32; i++) {
            // Shift result left to make room for the next bit
            res = res << 1;
            
            // Add the last bit of n to result
            // (n & 1) extracts the rightmost bit
            res = res | (n & 1);
            
            // Logically right shift n by 1
            // Use >>> to treat n as unsigned (fills with 0, not sign bit)
            n = n >>> 1;
        }
        
        return res;
    }
}
```

---

## Intuition

The goal is to move the last bit of $n$ to the first position of the result, the second-to-last bit of $n$ to the second position of the result, and so on.

### Bit-by-Bit Reconstruction
1.  We initialize `res` to 0.
2.  We run a loop exactly 32 times (since the input is a 32-bit integer).
3.  In each iteration:
    *   We shift our result `res` left by 1 position.
    *   We take the rightmost bit of `n` using `n & 1`.
    *   We OR this bit into the last position of `res`.
    *   We shift `n` right by 1 position using the logical shift operator `>>>`.
        *   **CRITICAL**: In Java, `>>` preserves the sign bit (arithmetic shift). Since we are treating $n$ as unsigned, we must use `>>>` which always shifts in a `0` from the left.

---

## Complexity Analysis

### Time Complexity

$O(1)$

- We always loop exactly 32 times, regardless of the input value.

### Space Complexity

$O(1)$

- We only use a single integer variable `res`.

---

## Key Takeaways

- Logical Shift (`>>>`) vs Arithmetic Shift (`>>`) is a vital distinction in Java bit manipulation.
- Reversing bits is effectively "re-threading" the bits from one end into another.
- Similar to reversing a decimal number using `% 10` and `* 10`, we use `& 1` and `<< 1` for bits.
