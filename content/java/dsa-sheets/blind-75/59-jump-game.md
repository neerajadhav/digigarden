---
title: 59 - Jump Game
tags:
  - Greedy
  - Array
  - Medium
---

You are given an integer array `nums` where each element `nums[i]` indicates your maximum jump length at that position.

Return `true` if you can reach the last index starting from index 0, or `false` otherwise.

---

## Examples

**Example 1**

Input: `nums = [1,2,0,1,0]`  
Output: `true`  
Explanation: First jump from index 0 to 1, then from index 1 to 3, and lastly from index 3 to 4.

**Example 2**

Input: `nums = [1,2,1,0,1]`  
Output: `false`

---

## Solution

### Java Code

```java
class Solution {
    public boolean canJump(int[] nums) {
        // We start from the last index as our initial goal
        int goal = nums.length - 1;

        // Iterate backwards from the second-to-last element to the first
        for (int i = nums.length - 2; i >= 0; i--) {
            // If we can reach the current 'goal' from position 'i'
            if (i + nums[i] >= goal) {
                // Update the goal to 'i'
                goal = i;
            }
        }

        // If the goal reaches index 0, then we can reach the end
        return goal == 0;
    }
}
```

---

## Intuition

The problem asks if it's *possible* to reach the end. While this can be solved with Dynamic Programming or BFS, a **Greedy** approach is the most efficient.

### Greedy Strategy
Instead of starting from the beginning and trying every jump (forward), we move **backwards** from the goal.
- We want to reach the end. Can the element before it reach the end? If yes, then our "new" goal becomes reaching *that* element.
- We keep shifting the target index closer to the start whenever we find a position from which we can jump to the current goal.

If at the end of the traversal the goal has reached the very first index (0), it means there is a path from 0 to the original last index.

---

## Complexity Analysis

### Time Complexity

$O(n)$

- We traverse the array exactly once from right to left.

### Space Complexity

$O(1)$

- We only use a single variable `goal` to track the target position.

---

## Key Takeaways

- Greedy works here because if we can reach an index $i$ that can reach the goal, we don't care *how* we reaches $i$; we just need to know $i$ is attainable.
- Backward iteration often simplifies "reachability" problems compared to forward simulation.
- This is significantly more efficient than a $O(n^2)$ DP solution where $dp[i]$ tracks if index $i$ is reachable.
