---
title: 72 - Counting Bits
tags:
  - Bit Manipulation
  - Dynamic Programming
  - Easy
---

Given an integer `n`, count the number of `1`'s in the binary representation of every number in the range `[0, n]`.

Return an array `res` of length `n + 1` where `res[i]` is the number of `1`'s in the binary representation of `i`.

---

## Examples

**Example 1**

Input: `n = 4`  
Output: `[0,1,1,2,1]`  
Explanation:  
`0` --> `0`  
`1` --> `1`  
`2` --> `10`  
`3` --> `11`  
`4` --> `100`

---

## Solution

### Java Code

```java
class Solution {
    public int[] countBits(int n) {
        int[] res = new int[n + 1];
        
        // res[0] is initialized to 0 by default
        for (int i = 1; i <= n; i++) {
            // Number of 1s in 'i' is:
            // (number of 1s in 'i / 2') + (1 if 'i' is odd, else 0)
            res[i] = res[i >> 1] + (i & 1);
        }
        
        return res;
    }
}
```

---

## Intuition

While we could use Brian Kernighan's algorithm on every number (costing $O(n \log n)$), we can solve this in $O(n)$ using **Dynamic Programming**.

### Transition Relation
Observe how shifting a number right affects its bit count:
- If we shift `i` right by one bit (`i >> 1`), we lose the last bit.
- The number of set bits in `i` is exactly the number of set bits in `i >> 1` plus the bit we just shifted off.
- The bit we shifted off is `1` if `i` is odd, and `0` if `i` is even. This is simply `i & 1`.

Therefore: `res[i] = res[i >> 1] + (i & 1)`.

---

## Complexity Analysis

### Time Complexity

$O(n)$

- We iterate from `1` to `n` exactly once, performing constant time operations in each step.

### Space Complexity

$O(1)$

- Not counting the output array, we use no extra auxiliary space.

---

## Key Takeaways

- Bit manipulation problems often have elegant DP solutions based on bit shifts or clearing the last set bit (`res[i] = res[i & (i - 1)] + 1`).
- The pattern of 1-bits is recursive and highly predictable.
- `i >> 1` is a very common optimization to reduce a problem into a previously solved subproblem in bit-related DP.
