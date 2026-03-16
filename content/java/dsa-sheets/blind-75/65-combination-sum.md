---
title: 65 - Combination Sum
tags:
  - Backtracking
  - Array
  - Medium
---

You are given an array of distinct integers `nums` and a target integer `target`. Your task is to return a list of all unique combinations of `nums` where the chosen numbers sum to `target`.

The same number may be chosen from `nums` an unlimited number of times.

---

## Examples

**Example 1**

Input: `nums = [2,5,6,9]`, `target = 9`  
Output: `[[2,2,5],[9]]`

**Example 2**

Input: `nums = [3,4,5]`, `target = 16`  
Output: `[[3,3,3,3,4],[3,3,5,5],[4,4,4,4],[3,4,4,5]]`

---

## Solution

### Java Code

```java
class Solution {
    public List<List<Integer>> combinationSum(int[] nums, int target) {
        List<List<Integer>> result = new ArrayList<>();
        backtrack(nums, target, 0, new ArrayList<>(), result);
        return result;
    }

    private void backtrack(int[] nums, int target, int start, List<Integer> current, List<List<Integer>> result) {
        // Base case: target is met
        if (target == 0) {
            result.add(new ArrayList<>(current));
            return;
        }

        // Base case: target exceeded
        if (target < 0) {
            return;
        }

        // Explore choices
        for (int i = start; i < nums.length; i++) {
            // Include nums[i]
            current.add(nums[i]);
            
            // Recurse with updated target. 
            // We pass 'i' as the new 'start' because we can reuse the same element.
            backtrack(nums, target - nums[i], i, current, result);
            
            // Backtrack: remove nums[i] before next iteration
            current.remove(current.size() - 1);
        }
    }
}
```

---

## Intuition

This problem is a classic **Backtracking** search. We need to explore all possible combinations of numbers that sum up to the target.

### Decision Tree
At each step, we can choose any number from the current index onwards (to avoid duplicate combinations like `[2, 3]` and `[3, 2]`).
- Because we can reuse numbers, when we recurse, we stay at the same index $i$.
- If the current sum equals the target, we've found a valid combination.
- If the current sum exceeds the target, we stop exploring that branch (pruning).

Using a `start` index is the key to ensuring **uniqueness**. By only looking forward in the array, we prevent revisiting previous numbers in a new combination.

---

## Complexity Analysis

### Time Complexity

$O(N^{\frac{T}{M} + 1})$

- Where $N$ is the number of candidates, $T$ is the target value, and $M$ is the minimal value among candidates.
- The execution can be visualized as an $N$-ary tree with a max depth of $T/M$.

### Space Complexity

$O(\frac{T}{M})$

- The space used by the recursion stack and the list to store the current combination.

---

## Key Takeaways

- Backtracking is the primary tool for "finding all combinations" problems.
- Passing the current index `i` into the recursive call allows for **reusing** elements.
- Always remember to copy the list (`new ArrayList<>(current)`) when adding it to the final result, as `current` will be modified in-place.
