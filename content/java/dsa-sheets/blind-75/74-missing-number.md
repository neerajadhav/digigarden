---
title: 74 - Missing Number
tags:
  - Bit Manipulation
  - Easy
---

Given an array `nums` containing `n` integers in the range `[0, n]` without any duplicates, return the single number in the range that is missing from `nums`.

Follow-up: Could you implement a solution using only $O(1)$ extra space complexity and $O(n)$ runtime complexity?

---

## Examples

**Example 1**

Input: `nums = [1,2,3]`  
Output: `0`  
Explanation: Since there are 3 numbers, the range is `[0,3]`. The missing number is `0` since it does not appear in `nums`.

**Example 2**

Input: `nums = [0,2]`  
Output: `1`

---

## Solution

### Java Code

```java
class Solution {
    public int missingNumber(int[] nums) {
        int n = nums.length;
        int res = n; // Start with n because the loop only goes up to n-1
        
        for (int i = 0; i < n; i++) {
            // XOR every index and every value
            // Every number from 0 to n-1 will appear twice (once as index, once as value)
            // EXCEPT the missing number and 'n'
            // We started res with 'n', so only the missing number will remain
            res = res ^ i ^ nums[i];
        }
        
        return res;
    }
}
```

---

## Intuition

We can solve this using the property of the **XOR** operator: $a \oplus a = 0$ and $a \oplus 0 = a$.

### XOR Strategy
If we XOR all the numbers from `0` to `n` and all the numbers present in the array, every number that is present twice will cancel out to 0. The only number that is present only once (the one from the range `0..n` that is not in the array) will be the result.

1.  Initialize `res` to `n` (the last number in the range).
2.  Iterate through the array:
    *   XOR `res` with the current index `i`.
    *   XOR `res` with the current value `nums[i]`.
3.  After the loop, `res` will hold the missing number.

---

## Complexity Analysis

### Time Complexity

$O(n)$

- We iterate through the array once.

### Space Complexity

$O(1)$

- We only use a single integer variable `res`.

---

## Key Takeaways

- XOR is a powerful tool for finding missing or unique elements in a collection.
- Another valid $O(1)$ space solution is using the **Sum Formula**: $\text{Sum}(0..n) = \frac{n(n+1)}{2}$. The missing number is $\text{TotalSum} - \text{ArraySum}$.
- XOR is generally preferred in systems where integer overflow might be a concern (though $n(n+1)/2$ is safe for $n \approx 10^4$ in a 32-bit int).
