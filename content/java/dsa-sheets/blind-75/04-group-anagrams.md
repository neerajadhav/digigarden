---
title: 04 - Group Anagrams
tags:
  - Array
  - String
  - Hashing
  - Medium
---

Given an array of strings `strs`, group all anagrams together into sublists. You may return the output in any order.

An anagram is a string that contains the exact same characters as another string, but the order of the characters can be different.

## Examples

Example 1

Input
```

strs = ["act","pots","tops","cat","stop","hat"]

```

Output
```

[["hat"],["act","cat"],["stop","pots","tops"]]

```

Example 2

Input
```

strs = ["x"]

```

Output
```

[["x"]]

```

Example 3

Input
```

strs = [""]

```

Output
```

[[""]]

````

## Solution

```java
import java.util.*;

class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {

        HashMap<String, List<String>> map = new HashMap<>();

        for (String s : strs) {

            char[] chars = s.toCharArray();
            Arrays.sort(chars);
            String key = new String(chars);

            map.putIfAbsent(key, new ArrayList<>());
            map.get(key).add(s);
        }

        return new ArrayList<>(map.values());
    }
}
````

## Intuition

Two strings are anagrams if their **sorted characters are identical**.

Example:

```
act -> act
cat -> act
```

Since both produce the same sorted representation, they belong to the same group.

Steps:

1. Traverse each word in the array.
    
2. Convert the word to a character array.
    
3. Sort the characters.
    
4. Use the sorted string as a key.
    
5. Store the original word in a list mapped to that key.
    

Example mapping:

```
"act"  -> ["act","cat"]
"opst" -> ["tops","pots","stop"]
"aht"  -> ["hat"]
```

Finally return all grouped values from the map.

## Java Collections Framework Components Used

### HashMap

Used to group anagrams efficiently.

Structure:

```
Key   -> sorted version of the word
Value -> list of words with the same sorted characters
```

Example:

```
"act"  -> ["act","cat"]
"opst" -> ["tops","pots","stop"]
```

Why `HashMap`:

- Fast lookup and insertion
    
- Allows grouping based on a computed key
    
- Average time complexity for operations is **O(1)**
    

Important methods used:

`putIfAbsent(key, value)`  
Creates a new list if the key does not exist.

`get(key)`  
Retrieves the list mapped to a key.

---

### ArrayList

Used to store multiple strings belonging to the same anagram group.

Example:

```
["tops","pots","stop"]
```

Why `ArrayList`:

- Dynamic size
    
- Efficient append operations
    

---

### Arrays.sort()

Used to sort characters in each string.

Example:

```
"stop" -> "opst"
```

The sorted string becomes the grouping key.

## Complexity Analysis

Let:

- `n` = number of strings
    
- `k` = maximum length of each string
    

Time Complexity

```
O(n * k log k)
```

Sorting each string takes `k log k`.

Space Complexity

```
O(n * k)
```

Space required for storing grouped strings in the hashmap.

## Key Takeaways

- Anagrams share the same **sorted character representation**.
    
- Use a `HashMap` to group strings efficiently.
    
- Key idea: **sorted string → list of anagrams**.
    
- Avoids expensive pairwise string comparisons.
    