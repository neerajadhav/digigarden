---
title: 16 - Valid Parentheses
tags:
  - String
  - Stack
  - Easy
---

You are given a string `s` consisting of the following characters: `(`, `)`, `{`, `}`, `[` and `]`.

The input string `s` is valid if and only if:
1. Every open bracket is closed by the same type of close bracket.
2. Open brackets are closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

Return `true` if `s` is a valid string, and `false` otherwise.

---

## Examples

**Example 1**

Input: 
```
s = "[]"
```

Output: 
```
true
```

---

**Example 2**

Input: 
```
s = "([{}])"
```

Output: 
```
true
```

---

**Example 3**

Input: 
```
s = "[(])"
```

Output: 
```
false
```

Explanation: The brackets are not closed in the correct order.

---

## Solution

### Java Code

```java
import java.util.HashMap;
import java.util.Map;
import java.util.Stack;

class Solution {
    public boolean isValid(String s) {
        // Map to store matching pairs
        Map<Character, Character> bracketMap = new HashMap<>();
        bracketMap.put(')', '(');
        bracketMap.put('}', '{');
        bracketMap.put(']', '[');

        // Stack to store open brackets
        Stack<Character> stack = new Stack<>();

        for (char c : s.toCharArray()) {
            // If it's a closing bracket
            if (bracketMap.containsKey(c)) {
                // If stack is empty or doesn't match top, return false
                if (stack.isEmpty() || stack.pop() != bracketMap.get(c)) {
                    return false;
                }
            } else {
                // It's an opening bracket, push to stack
                stack.push(c);
            }
        }

        // String is valid only if stack is empty
        return stack.isEmpty();
    }
}
```

---

## Java Collections Framework Components Used

## 1. `Stack`

### What it is
`Stack` is a class in the Java Collections Framework that represents a last-in, first-out (LIFO) stack of objects.

### How it is used
```java
Stack<Character> stack = new Stack<>();
```
Used to keep track of open brackets as we encounter them. When we see a closing bracket, we check if it matches the most recently opened bracket at the top of the stack.

### Key Methods Used:
- `push(E item)`: Pushes an item onto the top of this stack.
- `pop()`: Removes and returns the object at the top of this stack.
- `isEmpty()`: Tests if this stack is empty.

---

## 2. `HashMap`

### What it is
`HashMap` is a hash-table based implementation of the `Map` interface.

### How it is used
```java
Map<Character, Character> bracketMap = new HashMap<>();
```
Used to store the mapping between closing brackets and their corresponding opening brackets. This simplifies the matching logic in the loop.

---

## Intuition

The LIFO nature of parentheses matching makes a **Stack** the perfect data structure.

1. **Opening Brackets**: When we see `(`, `{`, or `[`, we push it onto the stack. These are "unresolved" brackets waiting for their partners.
2. **Closing Brackets**: When we see `)`, `}`, or `]`, they MUST match the most recently added open bracket. 
   - We check the top of the stack (`pop()`). 
   - If they don't match, or if the stack is already empty, the string is invalid.
3. **Completion**: Once we finish traversing the string, the stack should be empty. If any brackets remain, it means they were never closed.

---

## Complexity Analysis

### Time Complexity

```
O(n)
```

We traverse the string exactly once, and each character results in a constant-time stack operation (`push` or `pop`).

---

### Space Complexity

```
O(n)
```

In the worst case (e.g., all opening brackets `(((((`), the stack will store all $n$ characters.

---

## Key Takeaways

- **Stack** is the standard tool for problems involving nested structures or "matching" pairs.
- Using a `HashMap` for bracket pairs makes the code cleaner and easier to extend (e.g., if new bracket types were added).
- Always check `stack.isEmpty()` before popping and at the very end.
