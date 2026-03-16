---
title: 04 - Generate Permutations
tags:
  - Backtracking
  - Arrays
  - Strings
---

# Generate Permutations

Generating permutations is a classic application of backtracking. It involves finding all possible arrangements of a given set of elements.

## Problem
Given a collection of numbers (or a string), return all possible permutations.

## 1. Permutations without Duplicates
When all input elements are unique, we can use a simple backtracking approach with a `used` array to track which elements are already in the current permutation.

### Intuition
At each step, we pick an unused element and add it to our current list. Once the list is full, we record it and backtrack to try other elements.

### Java Implementation
```java
import java.util.*;

public class Main {
    public static void backtrack(int[] nums, List<Integer> current, boolean[] used, List<List<Integer>> result) {
        if (current.size() == nums.length) {
            result.add(new ArrayList<>(current));
            return;
        }

        for (int i = 0; i < nums.length; i++) {
            if (!used[i]) {
                used[i] = true;
                current.add(nums[i]);
                backtrack(nums, current, used, result);
                current.remove(current.size() - 1);
                used[i] = false;
            }
        }
    }

    public static List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        backtrack(nums, new ArrayList<>(), new boolean[nums.length], result);
        return result;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        if (sc.hasNextInt()) {
            int n = sc.nextInt();
            int[] nums = new int[n];
            for (int i = 0; i < n; i++) nums[i] = sc.nextInt();
            System.out.println(permute(nums));
        }
        sc.close();
    }
}
```

## 2. Permutations with Duplicates
If the input contains duplicate elements (e.g., `[1, 1, 2]`), simply using a `used` array will generate duplicate permutations. To solve this, we can:
1. **Sort** the input.
2. Skip the current element if it's the same as the previous one **and** the previous one was not used in the current recursion.

### Java Implementation
```java
public static void backtrackDuplicates(int[] nums, List<Integer> current, boolean[] used, List<List<Integer>> result) {
    if (current.size() == nums.length) {
        result.add(new ArrayList<>(current));
        return;
    }

    for (int i = 0; i < nums.length; i++) {
        if (used[i]) continue;
        // Skip duplicates
        if (i > 0 && nums[i] == nums[i-1] && !used[i-1]) continue;

        used[i] = true;
        current.add(nums[i]);
        backtrackDuplicates(nums, current, used, result);
        current.remove(current.size() - 1);
        used[i] = false;
    }
}
```

## 3. String Permutations
The logic remains identical for strings. You can convert the string to a `char[]` and follow the same backtracking pattern.

## Sample Input and Output

### Input
```text
3
1 1 2
```

### Output
```text
[[1, 1, 2], [1, 2, 1], [2, 1, 1]]
```

## Complexity
| Case | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **All Unique** | $O(n \times n!)$ | $O(n)$ |
| **With Duplicates** | $O(n \times n!)$ | $O(n)$ |

## Key Takeaways
- Use a `used` array to track state in simple cases.
- **Sorting** is essential when dealing with duplicates to identify identical adjacent elements.
- Backtracking handles the state space naturally by exploring and undoing choices.
