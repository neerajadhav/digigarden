---
title: 45 - Alien Dictionary
tags:
  - Graph
  - BFS
  - Topological Sort
  - Hard
---

There is a foreign language which uses the latin alphabet, but the order among letters is not "a", "b", "c" ... "z" as in English.

You receive a list of non-empty strings `words` from the dictionary, where the words are sorted lexicographically based on the rules of this new language.

Derive the order of letters in this language. If the order is invalid, return an empty string. If there are multiple valid order of letters, return any of them.

---

## Examples

**Example 1**

Input: `["z","o"]`  
Output: `"zo"`  
Explanation: From "z" and "o", we know 'z' < 'o', so return "zo".

**Example 2**

Input: `["hrn","hrf","er","enn","rfnn"]`  
Output: `"hernf"`  
Explanation:
- from "hrn" and "hrf", we know 'n' < 'f'
- from "hrf" and "er", we know 'h' < 'e'
- from "er" and "enn", we know 'r' < 'n'
- from "enn" and "rfnn" we know 'e' < 'r'
- one possible solution is "hernf"

---

## Solution

### Java Code

```java
import java.util.*;

class Solution {
    public String foreignDictionary(String[] words) {
        // Step 0: Create Adjacency List and In-Degree Map
        Map<Character, List<Character>> adj = new HashMap<>();
        Map<Character, Integer> inDegree = new HashMap<>();

        // Initialize every character found in any word
        for (String word : words) {
            for (char c : word.toCharArray()) {
                adj.putIfAbsent(c, new ArrayList<>());
                inDegree.putIfAbsent(c, 0);
            }
        }

        // Step 1: Build the Graph
        for (int i = 0; i < words.length - 1; i++) {
            String w1 = words[i];
            String w2 = words[i+1];
            int len = Math.min(w1.length(), w2.length());

            // Special case: if w2 is a prefix of w1, it's invalid
            if (w1.length() > w2.length() && w1.startsWith(w2)) {
                return "";
            }

            for (int j = 0; j < len; j++) {
                char c1 = w1.charAt(j);
                char c2 = w2.charAt(j);
                if (c1 != c2) {
                    // Avoid duplicate edges
                    if (!adj.get(c1).contains(c2)) {
                        adj.get(c1).add(c2);
                        inDegree.put(c2, inDegree.get(c2) + 1);
                    }
                    break; // Only the first differing character gives order
                }
            }
        }

        // Step 2: Kahn's Algorithm (BFS for Topological Sort)
        Queue<Character> queue = new LinkedList<>();
        for (char c : inDegree.keySet()) {
            if (inDegree.get(c) == 0) {
                queue.offer(c);
            }
        }

        StringBuilder sb = new StringBuilder();
        while (!queue.isEmpty()) {
            char current = queue.poll();
            sb.append(current);

            for (char neighbor : adj.get(current)) {
                inDegree.put(neighbor, inDegree.get(neighbor) - 1);
                if (inDegree.get(neighbor) == 0) {
                    queue.offer(neighbor);
                }
            }
        }

        // Cycle detection: if sb length doesn't match available chars, return ""
        return sb.length() == inDegree.size() ? sb.toString() : "";
    }
}
```

---

## Intuition

The lexicographically sorted words provide a set of **precedence rules**. For example, if "alien" comes before "apple", the first differing character ('i' and 'p') tells us that 'i' must come before 'p' in the alien alphabet.

This relationship `c1 < c2` can be represented as a **directed edge** in a graph. The goal is to perform a **Topological Sort** to find a valid linear ordering of characters.

### Pruning and Edge Cases
1.  **Prefix Rule**: If word `A` is a prefix of word `B`, but word `A` appears after word `B` in the dictionary (e.g., `["abc", "ab"]`), the order is invalid.
2.  **Cycles**: If there's a circular dependency (e.g., `a < b`, `b < c`, `c < a`), no valid ordering exists.

---

## Complexity Analysis

### Time Complexity

$O(C)$

- $C$ is the total number of characters in all words combined.
- Building the adjacency list takes $O(C)$.
- Topological sort (BFS) takes $O(V + E)$, where $V$ is at most 26 (unique characters) and $E$ is at most $C$.

### Space Complexity

$O(V + E)$

- To store the adjacency list and in-degree map.
- $V \le 26$, so space is primarily bounded by the number of unique character dependencies discovered.

---

## Key Takeaways

- Lexicographical sorted lists are a natural candidate for **Directed Graph** representations.
- Determining character order is equivalent to finding a **Topological Sort**.
- Always check for the **prefix edge case** where a longer word incorrectly precedes its shorter prefix.
