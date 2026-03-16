---
title: 46 - Climbing Stairs
tags:
  - Dynamic Programming
  - Recursion
  - Memoization
  - Easy
---

You are given an integer `n` representing the number of steps to reach the top of a staircase. You can climb with either 1 or 2 steps at a time.

Return the number of distinct ways to climb to the top of the staircase.

---

## Examples

**Example 1**

Input: `n = 2`  
Output: `2`  
Explanation:
1. 1 + 1 = 2
2. 2 = 2

**Example 2**

Input: `n = 3`  
Output: `3`  
Explanation:
1. 1 + 1 + 1 = 3
2. 1 + 2 = 3
3. 2 + 1 = 3

---

## Solution

### Java Code

```java
class Solution {
    public int climbStairs(int n) {
        if (n <= 2) {
            return n;
        }

        int first = 1;
        int second = 2;

        for (int i = 3; i <= n; i++) {
            int current = first + second;
            first = second;
            second = current;
        }

        return second;
    }
}
```

---

## Intuition

To reach the $n^{th}$ step, you must have come from either the $(n-1)^{th}$ step or the $(n-2)^{th}$ step.

Therefore, the number of ways to reach step $n$ is the sum of the ways to reach step $n-1$ and step $n-2$.

$$f(n) = f(n-1) + f(n-2)$$

This is the exact same recurrence relation as the **Fibonacci sequence**.

### Dynamic Programming Approach
Instead of using recursion (which would be $O(2^n)$), we can use an iterative approach with just two variables to store previous states, reducing the space complexity to $O(1)$.

1.  **Base Cases**:
    - For $n=1$, there is 1 way.
    - For $n=2$, there are 2 ways.
2.  **Iterate**: From $n=3$ to the target $n$, calculate the current number of ways by adding the previous two.

---

## Complexity Analysis

### Time Complexity

$O(n)$

- We perform a single loop from 3 to $n$ to calculate the final result.

### Space Complexity

$O(1)$

- We only use a constant amount of extra space (`first`, `second`, and `current`).

---

## Key Takeaways

- Climbing stairs is the most fundamental example of **Dynamic Programming**.
- Recognition of subproblems ($n-1$ and $n-2$) is key to forming the DP relationship.
- Many DP problems can be optimized from $O(n)$ space down to $O(1)$ if we only care about the most recent results.
