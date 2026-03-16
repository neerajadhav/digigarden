---
title: 01 - Recursion Basics
tags:
  - Recursion
  - Basics
---

# Recursion Basics

Recursion is a fundamental concept where a function **calls itself** to solve a problem. It works on the principle Table of **Divide and Conquer**, breaking a complex problem into smaller, more manageable sub-problems of the same type.

## Problem
How to solve problems that have a repetitive, self-similar structure by reducing them to a base case.

## Intuition
Imagine you are standing in a line and want to know your position. You ask the person in front, "What is your position?". They ask the person in front of them, and so on. The process continues until it reaches the first person (the **Base Case**), who says "I am at position 1". The answer then "bubbles back" until it reaches you.

## Key Components of Recursion

For any recursive function to be successful, it must have:
1.  **Base Case**: The condition where the recursion stops. Without this, the function will call itself infinitely, leading to a `StackOverflowError`.
2.  **Recursive Step (Inductive Step)**: The part where the function calls itself with a modified (usually smaller) version of the original input.

## Memory Management: The Call Stack
Recursion uses a **Stack** data structure in memory.
-   Each time a function is called, a new **Stack Frame** is created.
-   The frame stores local variables and the return address.
-   Frames are removed (popped) only when the base case is reached and the function starts returning.

## Types of Recursion

### 1. Tail Recursion
The recursive call is the **last statement** executed by the function. There is nothing left to do after the call returns.
-   **Optimization**: Some compilers can optimize tail recursion into a loop (Tail Call Optimization), though standard Java (JVM) does not currently do this.

### 2. Head Recursion
The recursive call is at the **beginning** of the function. Processing of the current state happens *after* the recursive call returns.

### 3. Tree Recursion
A function calls itself **multiple times** within its body (e.g., Fibonacci, Merge Sort). This forms a tree-like structure of calls.

### 4. Nested Recursion
A recursive function passes a **recursive call as an argument** to another recursive call (e.g., Ackermann function).

## Recursion vs Iteration

| Feature | Recursion | Iteration |
| :--- | :--- | :--- |
| **Logic** | Solves via sub-problems | Solves via loops |
| **State** | Managed by Call Stack | Managed by local variables |
| **Memory** | Higher (Stack overhead) | Lower |
| **Speed** | Can be slower due to overhead | Generally faster |
| **Readability** | Often cleaner for complex trees | Cleaner for simple linear logic |

## Algorithm Steps (Example: Factorial)
1. If the input `n` is 0 or 1, return 1 (**Base Case**).
2. Otherwise, return `n * factorial(n - 1)` (**Recursive Step**).

## Pseudocode
```text
FUNCTION factorial(n)
    IF n equals 0 THEN
        RETURN 1
    ELSE
        RETURN n * factorial(n - 1)
    END IF
END FUNCTION
```

## Java Code using JCF
Following the project pattern for competitive programming/testing.

```java
import java.util.*;

public class Main {
    /**
     * Calculates factorial using recursion (Head Recursion example)
     */
    public static long factorial(int n) {
        // Base Case
        if (n <= 1) return 1;
        
        // Recursive Step
        return n * factorial(n - 1);
    }

    /**
     * Calculates factorial using Tail Recursion
     */
    public static long tailFactorial(int n, long accumulator) {
        // Base Case
        if (n <= 1) return accumulator;
        
        // Recursive Step (Last statement is the call)
        return tailFactorial(n - 1, n * accumulator);
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        if (sc.hasNextInt()) {
            int t = sc.nextInt();
            while (t-- > 0) {
                if (sc.hasNextInt()) {
                    int n = sc.nextInt();
                    // Using standard recursion
                    System.out.println("Factorial of " + n + " is: " + factorial(n));
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
2
5
3
```

### Output
```text
Factorial of 5 is: 120
Factorial of 3 is: 6
```

## Complexity
| Type | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **Factorial** | $O(n)$ | $O(n)$ (Due to Stack) |
| **Fibonacci (Tree)** | $O(2^n)$ | $O(n)$ (Depth of tree) |

## Example and Dry Run (Factorial 3)
1.  `factorial(3)` calls `factorial(2)` -> Stack: `[f(3)]`
2.  `factorial(2)` calls `factorial(1)` -> Stack: `[f(3), f(2)]`
3.  `factorial(1)` returns `1` (**Base Case**) -> Stack: `[f(3), f(2), f(1)]`
4.  `factorial(2)` receives `1`, returns `2 * 1 = 2`
5.  `factorial(3)` receives `2`, returns `3 * 2 = 6`

## Variations
-   **Memoization**: Saving results of recursive calls to avoid redundant work (Top-down DP).
-   **Backtracking**: Exploring one path, and if it fails, "backtracking" to the previous state to try another path.
-   **Mutual Recursion**: Two functions calling each other.
