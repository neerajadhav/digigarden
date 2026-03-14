---
title: LLM Instructions - Blind-75
draft: true
---

# Documentation Structure for Blind-75 Problems

When documenting LeetCode problems from the Blind-75 list, follow this granular structure to match existing notes.

## Required Sections

### 1. Frontmatter
Include the problem number, title, and tags (Category and Difficulty).
```yaml
---
title: XX - Problem Name
tags:
  - Category (e.g., Array)
  - Difficulty (Easy/Medium/Hard)
---
```

### 2. Problem Statement
A brief description of the problem, using bold text for key terms and markdown math/code for equations.

### 3. Examples
Section with Example 1, Example 2, etc. Each example should clearly state **Input**, **Output**, and an **Explanation**.

### 4. Solution
A section containing the Java Code.
- Use a `class Solution` structure (Standard LeetCode format).
- Avoid `class Main` unless explicitly asked for a test-case runner.

### 5. Intuition
Explain the key observation or the "magic" behind the solution.

### 6. Java Collections Framework Component Used (If applicable)
If the solution heavily uses a JCF component (like `HashMap`, `HashSet`, `PriorityQueue`):
- **What it is**: Brief definition.
- **Why it is used**: Justification for choosing this structure over others.
- **How it is used in the solution**: Breakdown of specific methods (e.g., `put`, `containsKey`).

### 7. Complexity Analysis
Clear sections for **Time Complexity** and **Space Complexity** using O-notation.

### 8. Key Takeaways
Bullet points summarizing the most important lessons from the problem.
