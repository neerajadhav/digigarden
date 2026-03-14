---
title: 08 - Longest Consecutive Sequence
tags:
  - Array
  - Hashing
  - Medium
---

Given an array of integers `nums`, return the length of the longest consecutive sequence of elements that can be formed.

A **consecutive sequence** is a sequence of elements in which each element is exactly 1 greater than the previous element. The elements do not have to be consecutive in the original array.

You must write an algorithm that runs in $O(n)$ time.

---

## Examples

**Example 1**

Input: 
```
nums = [2, 20, 4, 10, 3, 4, 5]
```

Output: 
```
4
```

Explanation: The longest consecutive sequence is `[2, 3, 4, 5]`.

---

**Example 2**

Input: 
```
nums = [0, 3, 2, 5, 4, 6, 1, 1]
```

Output: 
```
7
```

---

## Solution

### Java Code

```java
import java.util.HashSet;
import java.util.Set;

class Solution {
    public int longestConsecutive(int[] nums) {
        if (nums.length == 0) return 0;

        Set<Integer> set = new HashSet<>();
        for (int num : nums) {
            set.add(num);
        }

        int longest = 0;

        for (int num : set) {
            // Check if this is the start of a sequence
            // If num - 1 exists, then 'num' is not the start
            if (!set.contains(num - 1)) {
                int currentNum = num;
                int currentStreak = 1;

                // Count consecutive elements
                while (set.contains(currentNum + 1)) {
                    currentNum += 1;
                    currentStreak += 1;
                }

                longest = Math.max(longest, currentStreak);
            }
        }

        return longest;
    }
}
```

---

## Intuition

The challenge is to find the longest sequence in $O(n)$ time. A brute-force sorting approach would take $O(n \log n)$.

To achieve $O(n)$:
1. **Use a HashSet**: Store all unique numbers in a `HashSet`. This allows for $O(1)$ average time complexity for lookups.
2. **Identify the Start of a Sequence**: A number `num` is the start of a sequence only if `num - 1` is **not** in the set.
3. **Traverse and Count**: For every number that is a "start", increment and check the set for the next numbers (`num + 1`, `num + 2`, etc.) to find the length of that specific sequence.

**Why is it $O(n)$?**
Even though there's a `while` loop inside the `for` loop, each number is only "visited" as a start of a sequence once. The `while` loop only runs for elements that are parts of a sequence, and each element can be part of exactly one consecutive sequence. Thus, each element is processed at most twice.

---

## Java Collections Framework Component Used

## 1. `HashSet`

Used for $O(1)$ lookup to check existence of neighbors (`num - 1` and `num + 1`).

---

## Complexity Analysis

### Time Complexity

```
O(n)
```

We traverse the array once to build the set, and then iterate through the unique elements. Each element is visited at most twice.

---

### Space Complexity

```
O(n)
```

In the worst case, we store all `n` elements in the `HashSet`.

---

## Key Takeaways

- **HashSet for $O(1)$ lookups** is the standard way to turn search problems into linear time problems.
- **Skipping non-starts** is the crucial optimization that prevents the algorithm from becoming $O(n^2)$.
- The problem doesn't require elements to be unique or in order, but the "start" check handles duplicates and offsets naturally.
