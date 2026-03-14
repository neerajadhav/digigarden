---
title: 02 - Valid Anagrams
tags:
  - Array
  - Hashing
  - Easy
  - String
---

Given two strings `s` and `t`, return `true` if the two strings are **anagrams of each other**, otherwise return `false`.

An **anagram** is a string that contains **exactly the same characters with the same frequency**, but the **order of characters can be different**.

---

## Examples

**Example 1**

Input

```
s = "racecar"
t = "carrace"
```

Output

```
true
```

Explanation  
Both strings contain the same characters: `r, a, c, e` with the same frequencies.

---

**Example 2**

Input

```
s = "jar"
t = "jam"
```

Output

```
false
```

Explanation  
The characters differ (`r` vs `m`), so they are not anagrams.

---

## Solution

### Java Code

```java
import java.util.HashMap;

class Solution {
    public boolean isAnagram(String s, String t) {

        if (s.length() != t.length()) {
            return false;
        }

        HashMap<Character, Integer> map = new HashMap<>();

        for (char c : s.toCharArray()) {
            map.put(c, map.getOrDefault(c, 0) + 1);
        }

        for (char c : t.toCharArray()) {

            if (!map.containsKey(c)) {
                return false;
            }

            map.put(c, map.get(c) - 1);

            if (map.get(c) == 0) {
                map.remove(c);
            }
        }

        return map.isEmpty();
    }
}
```

---

## Intuition

Two strings are anagrams if:

1. They have **the same length**
    
2. They have **the same characters**
    
3. Each character appears **the same number of times**
    

So the idea is:

1. Count the frequency of each character in string `s`.
    
2. Reduce the frequency using characters from string `t`.
    
3. If every character cancels out perfectly, the map will be empty.
    

Steps:

1. If lengths differ → immediately return `false`.
    
2. Build a frequency map from `s`.
    
3. Traverse `t` and decrement counts.
    
4. If a character is missing or frequency mismatch occurs → return `false`.
    
5. If map becomes empty → strings are anagrams.
    

---

## Java Collections Framework Component Used

## 1. `HashMap`

### What it is

`HashMap` is a **key-value based data structure** in the Java Collections Framework.

It stores **pairs of keys and values**.

In this problem:

```
Key   → Character
Value → Frequency of character
```

Example

```
racecar
```

Frequency map becomes:

```
r → 2
a → 2
c → 2
e → 1
```

---

### Why `HashMap` is used

`HashMap` is used because:

- It allows **efficient frequency counting**
    
- It provides **O(1) average lookup time**
    
- It can store **character → frequency mapping**
    

Time complexity for operations:

|Operation|Time Complexity|
|---|---|
|put()|O(1)|
|get()|O(1)|
|containsKey()|O(1)|
|remove()|O(1)|

---

### How it is used in the solution

### 1. Creating the Map

```java
HashMap<Character, Integer> map = new HashMap<>();
```

This stores character frequencies.

---

### 2. Counting Characters

```java
map.put(c, map.getOrDefault(c, 0) + 1);
```

`getOrDefault()` is a useful JCF method.

Meaning:

- If key exists → return its value
    
- If key does not exist → return default value
    

Example

```
map.getOrDefault('a', 0)
```

---

### 3. Checking Characters in Second String

```java
if (!map.containsKey(c)) {
    return false;
}
```

If a character from `t` doesn't exist in `s`, they cannot be anagrams.

---

### 4. Decreasing Frequency

```java
map.put(c, map.get(c) - 1);
```

Each matching character cancels one occurrence.

---

### 5. Removing Zero Frequency

```java
if (map.get(c) == 0) {
    map.remove(c);
}
```

When frequency becomes zero, the key is removed.

---

### 6. Final Check

```java
return map.isEmpty();
```

If all characters cancel out, the map becomes empty.

---

## Complexity Analysis

### Time Complexity

```
O(n)
```

Where `n` is the length of the string.

We traverse both strings once.

---

### Space Complexity

```
O(k)
```

Where `k` is the number of **unique characters**.

For lowercase English letters, `k ≤ 26`.

---

## Key Takeaways

- **Anagrams require equal character frequency.**
    
- `HashMap` is ideal for **frequency counting problems**.
    
- `getOrDefault()` is commonly used in JCF for counting patterns.
    
- This solution runs in **linear time O(n)** and is optimal.
    
