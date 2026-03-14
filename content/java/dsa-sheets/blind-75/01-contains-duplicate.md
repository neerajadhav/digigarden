---
title: 01 - Contains Duplicate
tags:
  - Array
  - Hashing
  - Easy
---

Given an integer array `nums`, return `true` if **any value appears more than once** in the array, otherwise return `false`.

In other words, check whether the array contains **duplicate elements**.

---

## Examples

**Example 1**

Input

```
nums = [1, 2, 3, 3]
```

Output

```
true
```

Explanation  
`3` appears twice, so the array contains duplicates.

---

**Example 2**

Input

```
nums = [1, 2, 3, 4]
```

Output

```
false
```

Explanation  
All elements are unique.

---

## Solution

### Java Code

```java
import java.util.HashSet;

class Solution {
    public boolean hasDuplicate(int[] nums) {

        HashSet<Integer> set = new HashSet<>();

        for (int num : nums) {

            if (!set.add(num)) {
                return true;
            }

        }

        return false;
    }
}
```

---

## Intuition

The key idea is to **track elements that have already appeared**.

If we encounter a number that we have already seen before, it means the array contains a duplicate.

Steps:

1. Traverse the array.
    
2. Maintain a collection of elements that have already been seen.
    
3. For each number:
    
    - Try to add it to the collection.
        
    - If it **already exists**, then a duplicate is found.
        
4. If traversal completes without duplicates, return `false`.
    

This approach avoids nested loops and allows checking duplicates efficiently.

---

## Java Collections Framework Component Used

## 1. `HashSet`

### What it is

`HashSet` is a **collection that stores unique elements only**.

It is part of the **Java Collections Framework (JCF)** and is implemented using a **hash table**.

Import statement:

```java
import java.util.HashSet;
```

---

### Why `HashSet` is used

`HashSet` is ideal for this problem because:

- It **automatically prevents duplicate elements**.
    
- It provides **fast lookup**.
    
- It allows checking duplicates in **constant time**.
    

Time complexity for operations:

|Operation|Time Complexity|
|---|---|
|add()|O(1) average|
|contains()|O(1) average|

---

### How it is used in the solution

```java
HashSet<Integer> set = new HashSet<>();
```

A set is created to store elements we have already seen.

---

### Key Operation Used

#### `add(E element)`

```java
set.add(num)
```

Important behavior:

- Returns **true** if the element was added successfully.
    
- Returns **false** if the element **already exists in the set**.
    

Example:

```java
set.add(3); // true
set.add(3); // false
```

In the algorithm:

```java
if (!set.add(num)) {
    return true;
}
```

Meaning:

- If `add()` returns `false`, the element already exists → **duplicate found**.
    

---

## Complexity Analysis

### Time Complexity

```
O(n)
```

Where `n` is the number of elements in the array.

Each element is processed once.

---

### Space Complexity

```
O(n)
```

In the worst case, all elements are unique and stored in the `HashSet`.

---

## Key Takeaways

- `HashSet` is the **most common JCF tool for duplicate detection**.
    
- `add()` returning `false` is a **clean trick to detect duplicates**.
    
- This solution avoids expensive **O(n²)** comparisons.
    
- The approach is **simple, readable, and optimal for this problem**.