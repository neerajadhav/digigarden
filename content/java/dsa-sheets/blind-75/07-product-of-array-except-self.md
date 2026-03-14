---
title: 07 - Products of Array Except Self
tags:
  - Array
  - Prefix-Postfix
  - Medium
---

Given an integer array `nums`, return an array `output` where `output[i]` is the product of all the elements of `nums` except `nums[i]`.

Each product is guaranteed to fit in a 32-bit integer.

**Follow-up:** Could you solve it in $O(n)$ time without using the division operation?

---

## Examples

**Example 1**

Input: 
```
nums = [1, 2, 4, 6]
```

Output: 
```
[48, 24, 12, 8]
```

---

**Example 2**

Input: 
```
nums = [-1, 0, 1, 2, 3]
```

Output: 
```
[0, -6, 0, 0, 0]
```

---

## Solution

### Java Code

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] res = new int[n];

        // Step 1: Calculate prefix products
        // res[i] will store the product of all elements to the left of nums[i]
        res[0] = 1;
        for (int i = 1; i < n; i++) {
            res[i] = res[i - 1] * nums[i - 1];
        }

        // Step 2: Calculate postfix products on the fly and multiply with prefix
        // rightProduct will track the product of all elements to the right of nums[i]
        int rightProduct = 1;
        for (int i = n - 1; i >= 0; i--) {
            res[i] = res[i] * rightProduct;
            rightProduct *= nums[i];
        }

        return res;
    }
}
```

---

## Intuition

The goal is to calculate the product of all elements except the current one without using division.

The product for any element at index `i` can be seen as:
`(Product of all elements to its left) * (Product of all elements to its right)`

1. **Prefix Products**: We traverse the array from left to right, storing the cumulative product of all elements seen so far in the result array. For `res[i]`, this is `nums[0] * ... * nums[i-1]`.
2. **Postfix Products**: We then traverse the array from right to left. We keep track of a "product from the right" and multiply it with the value already in `res[i]` (which is the prefix product).

This allows us to compute the final result in two passes without extra space besides the output array.

---

## Complexity Analysis

### Time Complexity

```
O(n)
```

We traverse the array twice: once to compute prefix products and once to compute postfix products.

---

### Space Complexity

```
O(1)
```

(Ignoring the space required for the output array). We only use a single variable `rightProduct` for intermediate calculations.

---

## Key Takeaways

- **Prefix/Postfix pattern** is a powerful technique for problems involving range products or sums without using total sum/product.
- Handling $O(n)$ time without division is often a hint toward this two-pass approach.
- The space complexity can be optimized by reusing the output array for one of the passes.
