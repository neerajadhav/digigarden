---
title: 37 - Design Add and Search Words Data Structure
tags:
  - Trie
  - Design
  - Backtracking
  - Medium
---

Design a data structure that supports adding new words and searching for existing words.

Implement the `WordDictionary` class:

- `void addWord(word)` Adds `word` to the data structure.
- `bool search(word)` Returns `true` if there is any string in the data structure that matches `word` or `false` otherwise. `word` may contain dots `'.'` where dots can be matched with any letter.

---

## Examples

**Example 1**

Input:

```
["WordDictionary","addWord","addWord","addWord","search","search","search","search"]
[[],["day"],["bay"],["may"],["say"],["day"],[".ay"],["b.."]]
```

Output:

```
[null, null, null, null, false, true, true, true]
```

Explanation:

```java
WordDictionary wordDictionary = new WordDictionary();
wordDictionary.addWord("day");
wordDictionary.addWord("bay");
wordDictionary.addWord("may");
wordDictionary.search("say"); // return false
wordDictionary.search("day"); // return true
wordDictionary.search(".ay"); // return true
wordDictionary.search("b.."); // return true
```

---

## Solution

### Java Code

```java
class WordDictionary {
    private class TrieNode {
        TrieNode[] children = new TrieNode[26];
        boolean isEndOfWord = false;
    }

    private final TrieNode root;

    public WordDictionary() {
        root = new TrieNode();
    }

    public void addWord(String word) {
        TrieNode current = root;
        for (char ch : word.toCharArray()) {
            int index = ch - 'a';
            if (current.children[index] == null) {
                current.children[index] = new TrieNode();
            }
            current = current.children[index];
        }
        current.isEndOfWord = true;
    }

    public boolean search(String word) {
        return dfs(word, 0, root);
    }

    private boolean dfs(String word, int index, TrieNode current) {
        if (index == word.length()) {
            return current.isEndOfWord;
        }

        char ch = word.charAt(index);
        if (ch == '.') {
            // Wildcard: search all possible branches
            for (TrieNode child : current.children) {
                if (child != null && dfs(word, index + 1, child)) {
                    return true;
                }
            }
            return false;
        } else {
            // Exact match
            int charIndex = ch - 'a';
            TrieNode child = current.children[charIndex];
            if (child == null) {
                return false;
            }
            return dfs(word, index + 1, child);
        }
    }
}
```

---

## Intuition

This problem is a variation of the standard Trie search. The "dot" `.` character acts as a **wildcard** that matches any of the 26 possible children.

When we encounter a `.` during search, we can't just pick one branch. Instead, we must **systematically explore all existing child nodes** at that level. If any of those branches lead to a successful match for the rest of the string, we return `true`.

### Key Mechanism: DFS (Backtracking)

- **Standard Search**: Follow a single path down the tree.
- **Wildcard Search**: At each `.` index, perform a depth-first search (DFS) through all 26 potential child nodes.

---

## Complexity Analysis

### Time Complexity

- **addWord**: $O(L)$, where $L$ is the length of the word.
- **search**:
    - **Best Case (No dots)**: $O(L)$
    - **Worst Case (All dots)**: $O(M \times 26^L)$ in theory, but practically capped by the total number of nodes $N$. Since at most 2 dots are allowed per query (per constraints), the search is quite efficient.

### Space Complexity

- **$O(N \times L)$**: To store all characters in the Trie.

---

## Key Takeaways

- Use a **Trie** for prefix and word search efficiency.
- Handle wildcards using **recursion/DFS**.
- Tries are efficient because they **minimize redundant string storage** by sharing prefixes.
