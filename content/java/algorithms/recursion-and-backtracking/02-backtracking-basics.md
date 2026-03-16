---
title: 02 - Backtracking Basics
tags:
  - Backtracking
  - Basics
  - Recursion
---

# Backtracking Basics

Backtracking is a refined algorithmic technique that uses **recursion** to explore all possible solutions to a problem. It follows a "trial and error" approach, where you build a solution incrementally and "backtrack" (undo the last step) as soon as you realize the current path cannot lead to a valid solution.

## Problem
How to find solutions in a massive search space without visiting every single possibility (Pruning).

## Intuition
Think of solving a **Maze**.
1. You take a path (Choice).
2. If you hit a dead end (Constraint violation), you go back to the last junction (Backtrack).
3. You try a different path (Explore).

## The Three Pillars of Backtracking

Every backtracking problem can be broken down into these three elements:
1.  **Choice**: What is the next step we can take? (e.g., Which number to place in Sudoku?)
2.  **Constraints**: What rules must this choice follow? (e.g., No duplicate numbers in a row.)
3.  **Goal**: When is the solution complete? (e.g., All cells are filled correctly.)

## State Space Tree
Backtracking can be visualized as a **Depth First Search (DFS)** traversal on a virtual tree called the **State Space Tree**.
-   **Nodes**: Represent partial solutions.
-   **Edges**: Represent choices made to reach the next state.
-   **Pruning**: Cutting off branches of the tree that are guaranteed not to lead to a solution.

## Backtracking vs Brute Force

| Feature | Brute Force | Backtracking |
| :--- | :--- | :--- |
| **Approach** | Generates all possible candidates | Generates candidates incrementally |
| **Efficiency** | Low (visits everything) | Higher (uses pruning) |
| **Logic** | Simple loops | Recursion + State management |

## Algorithm Template
```text
FUNCTION backtrack(state):
    IF goal reached THEN
        save state
        RETURN

    FOR each choice in available_choices:
        IF choice is valid (Constraints):
            make choice (Update state)
            backtrack(state)
            undo choice (BACKTRACK - restore state)
```

## Java Code using JCF
Example: Generating all **Permutations** of a list of integers.

```java
import java.util.*;

public class Main {
    /**
     * Generates all permutations of a list
     */
    public static void findPermutations(List<Integer> nums, List<Integer> current, boolean[] used, List<List<Integer>> result) {
        // Goal: If current permutation length equals input length
        if (current.size() == nums.size()) {
            result.add(new ArrayList<>(current));
            return;
        }

        for (int i = 0; i < nums.size(); i++) {
            // Constraint: Each element used once per permutation
            if (!used[i]) {
                // 1. Make Choice
                used[i] = true;
                current.add(nums.get(i));

                // 2. Explore
                findPermutations(nums, current, used, result);

                // 3. Backtrack (Undo Choice)
                used[i] = false;
                current.remove(current.size() - 1);
            }
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        if (sc.hasNextInt()) {
            int t = sc.nextInt();
            while (t-- > 0) {
                int n = sc.nextInt();
                List<Integer> nums = new ArrayList<>();
                for (int i = 0; i < n; i++) {
                    nums.add(sc.nextInt());
                }

                List<List<Integer>> result = new ArrayList<>();
                findPermutations(nums, new ArrayList<>(), new boolean[n], result);
                
                System.out.println("Total Permutations: " + result.size());
                for (List<Integer> perm : result) {
                    System.out.println(perm);
                }
            }
        }
        sc.close();
    }
}
```

## Sample Input and Output

### Input
```text
1
3
1 2 3
```

### Output
```text
Total Permutations: 6
[1, 2, 3]
[1, 3, 2]
[2, 1, 3]
[2, 3, 1]
[3, 1, 2]
[3, 2, 1]
```

## Complexity
| Type | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **Permutations** | $O(n \times n!)$ | $O(n)$ (Recursion depth) |
| **N-Queens** | $O(n!)$ | $O(n)$ |
| **Subsets** | $O(2^n)$ | $O(n)$ |

## Key Takeaways
- Always **undo** the changes made to the state before returning from a recursive call.
- Effective **pruning** is the secret to fast backtracking algorithms.
- Backtracking is the foundation for solving puzzles like Sudoku, N-Queens, and Crosswords.
