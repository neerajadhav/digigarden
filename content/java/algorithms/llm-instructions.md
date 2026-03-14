---
title: LLM Instructions - Algorithms
draft: true
---

# Documentation Structure for Algorithms

When documenting a new algorithm, follow this exact structure to ensure consistency.

## Required Sections

### 1. Problem
Clear description of what the algorithm solves.

### 2. Intuition
The core idea, metaphors (e.g., "bubbles" for Bubble Sort), or the basic logic behind the approach.

### 3. Algorithm Steps
A step-by-step breakdown of the process.

### 4. Pseudocode
A language-agnostic procedural representation.

### 5. Java Code using JCF
Use a competitive programming template:
- Use `Scanner` for input.
- Handle multiple test cases with `while (t-- > 0)`.
- The public class must be named `Main`.
- Use JCF (Java Collections Framework) like `ArrayList`, `Collections.swap`, etc. where appropriate.

```java
import java.util.*;

public class Main {
    public static void algorithm(List<Integer> list) {
        // Implementation
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        if (sc.hasNextInt()) {
            int t = sc.nextInt();
            while (t-- > 0) {
                // Read N and build List/Array
                // Execute algorithm
                // Print result in a single line
            }
        }
        sc.close();
    }
}
```

### 6. Sample Input and Output
A single block showing the target input/output format.

### 7. Complexity
A table for Time (Best, Average, Worst) and Space complexity. Note if it is **Stable** or **In-place**.

### 8. Example and Dry Run
A detailed trace of a specific example.

### 9. Variations
Related algorithms or optimizations (e.g., Cocktail Sort, Bidirectional Selection Sort).
