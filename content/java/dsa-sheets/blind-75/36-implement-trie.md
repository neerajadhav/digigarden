---
title: 36 - Implement Trie (Prefix Tree)
tags:
  - Trie
  - Design
  - Medium
---

A **prefix tree** (also known as a **trie**) is a tree data structure used to efficiently store and retrieve keys in a set of strings. Some applications of this data structure include auto-complete and spell checker systems.

Implement the `PrefixTree` class:

- `PrefixTree()` Initializes the prefix tree object.
- `void insert(String word)` Inserts the string `word` into the prefix tree.
- `boolean search(String word)` Returns `true` if the string `word` is in the prefix tree (i.e., was inserted before), and `false` otherwise.
- `boolean startsWith(String prefix)` Returns `true` if there is a previously inserted string `word` that has the prefix `prefix`, and `false` otherwise.

---

## Examples

**Example 1**

Input:

```
["Trie", "insert", "dog", "search", "dog", "search", "do", "startsWith", "do", "insert", "do", "search", "do"]
```

Output:

```
[null, null, true, false, true, null, true]
```

Explanation:

```java
PrefixTree prefixTree = new PrefixTree();
prefixTree.insert("dog");
prefixTree.search("dog");    // return true
prefixTree.search("do");     // return false
prefixTree.startsWith("do"); // return true
prefixTree.insert("do");
prefixTree.search("do");     // return true
```

---

## Solution

### Java Code

```java
class PrefixTree {
    // Inner class representing each node in the Trie
    private class TrieNode {
        TrieNode[] children = new TrieNode[26];
        boolean isEndOfWord = false;
    }

    private final TrieNode root;

    /** Initializes the prefix tree object. */
    public PrefixTree() {
        root = new TrieNode();
    }

    /** Inserts the string word into the prefix tree. */
    public void insert(String word) {
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

    /** Returns true if the word is in the prefix tree. */
    public boolean search(String word) {
        TrieNode node = find(word);
        return node != null && node.isEndOfWord;
    }

    /** Returns true if any word starts with the given prefix. */
    public boolean startsWith(String prefix) {
        return find(prefix) != null;
    }

    // Helper method to navigate to the end node of a string
    private TrieNode find(String s) {
        TrieNode current = root;
        for (char ch : s.toCharArray()) {
            int index = ch - 'a';
            if (current.children[index] == null) {
                return null;
            }
            current = current.children[index];
        }
        return current;
    }
}
```

---

## Intuition

The core idea of a Trie is to **store strings character by character** in a tree-like structure, where each node represents a single character.

This allows us to share common prefixes among different strings, saving space and enabling efficient prefix-based lookups.

### Steps:

1.  **Node Structure**: Each node contains an array of children (for each possible character) and a flag to mark the end of a word.
2.  **Insertion**: Traverse the tree character by character. If a child node doesn't exist for the current character, create one.
3.  **Search**: Traverse the tree. If any character in the word is missing, the word doesn't exist. If the path exists, check the `isEndOfWord` flag at the last character.
4.  **Prefix Check**: Similar to search, but only check if the path exists for the given prefix.

---

## Complexity Analysis

### Time Complexity

| Operation | Time Complexity |
| :--- | :--- |
| `insert()` | $O(L)$ |
| `search()` | $O(L)$ |
| `startsWith()` | $O(P)$ |

*   $L$ is the length of the word, $P$ is the length of the prefix.

### Space Complexity

$O(N \times L)$, where $N$ is the number of words and $L$ is the average length. In the worst case, every character of every word is stored in a separate node.

---

## Key Takeaways

- Tries are the **go-to data structure for prefix-related problems**.
- They provide **predictable performance** independent of the number of words stored.
- Space efficiency comes from **prefix sharing**.
