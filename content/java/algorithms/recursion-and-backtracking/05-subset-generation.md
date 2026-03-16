---
title: 05 - Subset Generation
tags:
  - Backtracking
  - Recursion
  - Bit Manipulation
---

# Subset Generation (Power Set)

Generating all subsets of a set (the **Power Set**) is a fundamental problem that can be solved using several different paradigms.

## Problem
Given an integer array `nums` of unique elements, return all possible subsets (the power set).

## 1. Recursive Approach (Pick / Don't Pick)
This is the most intuitive approach. For every element, we have two choices:
1.  **Include** it in the current subset.
2.  **Exclude** it from the current subset.

### Java Implementation
```java
import java.util.*;

public class Main {
    public static void generateSubsets(int[] nums, int index, List<Integer> current, List<List<Integer>> result) {
        if (index == nums.length) {
            result.add(new ArrayList<>(current));
            return;
        }

        // Choice 1: Include nums[index]
        current.add(nums[index]);
        generateSubsets(nums, index + 1, current, result);

        // Choice 2: Exclude nums[index] (Backtrack)
        current.remove(current.size() - 1);
        generateSubsets(nums, index + 1, current, result);
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        if (sc.hasNextInt()) {
            int n = sc.nextInt();
            int[] nums = new int[n];
            for (int i = 0; i < n; i++) nums[i] = sc.nextInt();
            
            List<List<Integer>> result = new ArrayList<>();
            generateSubsets(nums, 0, new ArrayList<>(), result);
            System.out.println(result);
        }
        sc.close();
    }
}
```

## 2. Backtracking Approach (Loop-based)
This approach uses a loop to pick the "next" element and recursive calls to explore deeper. It is often preferred for problems where you need to find subsets of a specific size or with specific properties.

### Java Implementation
```java
public static void backtrack(int[] nums, int start, List<Integer> current, List<List<Integer>> result) {
    result.add(new ArrayList<>(current));

    for (int i = start; i < nums.length; i++) {
        current.add(nums[i]);
        backtrack(nums, i + 1, current, result);
        current.remove(current.size() - 1);
    }
}
```

## 3. Subsets with Duplicates (Subsets II)
If the input contains duplicates, we first **sort** the array and skip duplicates in the loop-based backtracking approach.

### Java Implementation
```java
public static void backtrackDuplicates(int[] nums, int start, List<Integer> current, List<List<Integer>> result) {
    result.add(new ArrayList<>(current));

    for (int i = start; i < nums.length; i++) {
        // Skip duplicates
        if (i > start && nums[i] == nums[i-1]) continue;
        
        current.add(nums[i]);
        backtrackDuplicates(nums, i + 1, current, result);
        current.remove(current.size() - 1);
    }
}
```

## 4. Bit Manipulation Approach
For a set of size $n$, there are $2^n$ subsets. We can map each number from $0$ to $2^n - 1$ to a subset where the $i$-th bit indicates whether the $i$-th element is included.

### Java Implementation
```java
public static List<List<Integer>> subsetsBitmask(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    int n = nums.length;
    int total = 1 << n; // 2^n

    for (int i = 0; i < total; i++) {
        List<Integer> subset = new ArrayList<>();
        for (int j = 0; j < n; j++) {
            if ((i & (1 << j)) != 0) {
                subset.add(nums[j]);
            }
        }
        result.add(subset);
    }
    return result;
}
```

## Sample Input and Output

### Input
```text
3
1 2 3
```

### Output
```text
[[], [1], [1, 2], [1, 2, 3], [1, 3], [2], [2, 3], [3]]
```

## Complexity
| Approach | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **Recursive/Backtracking** | $O(n \times 2^n)$ | $O(n)$ (Recursion depth) |
| **Bit Manipulation** | $O(n \times 2^n)$ | $O(1)$ (Excluding result) |

## Key Takeaways
- The total number of subsets is always $2^n$.
- **Backtracking** is versatile and easy to adapt for constraints.
- **Bit Manipulation** is highly efficient for small $n$ ($n \le 31$ for `int`).
