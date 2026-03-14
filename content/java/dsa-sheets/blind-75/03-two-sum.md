---
title: 03 - Two Sum
tags:
  - Array
  - Hashing
  - Easy
---

Given an integer array `nums` and an integer `target`, return the **indices** `i` and `j` such that:

```
nums[i] + nums[j] == target
```

Conditions:

- `i != j`
    
- Exactly **one valid pair exists**
    
- Return the **smaller index first**
    

---

## Examples

**Example 1**

Input

```
nums = [3,4,5,6]
target = 7
```

Output

```
[0,1]
```

Explanation  
`nums[0] + nums[1] = 3 + 4 = 7`

---

**Example 2**

Input

```
nums = [4,5,6]
target = 10
```

Output

```
[0,2]
```

Explanation  
`nums[0] + nums[2] = 4 + 6 = 10`

---

**Example 3**

Input

```
nums = [5,5]
target = 10
```

Output

```
[0,1]
```

---

## Solution

### Java Code (Optimal O(n) Approach)

```java
import java.util.HashMap;

class Solution {
    public int[] twoSum(int[] nums, int target) {

        HashMap<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < nums.length; i++) {

            int complement = target - nums[i];

            if (map.containsKey(complement)) {
                return new int[]{map.get(complement), i};
            }

            map.put(nums[i], i);
        }

        return new int[]{};
    }
}
```

---

## Intuition

The key observation:

```
a + b = target
```

Which means:

```
b = target - a
```

So while iterating through the array:

1. For each number `nums[i]`
    
2. Compute the **complement** needed:
    

```
complement = target - nums[i]
```

3. Check if that complement already exists in the data structure.
    
4. If it exists → we found the required pair.
    

Example:

```
nums = [3,4,5,6]
target = 7
```

Iteration:

|i|nums[i]|complement|map|result|
|---|---|---|---|---|
|0|3|4|{}|store 3|
|1|4|3|{3:0}|found pair|

Return:

```
[0,1]
```

---

## Java Collections Framework Component Used

### `HashMap`

#### What it is

`HashMap` is a **key-value based data structure** from the Java Collections Framework.

It allows **fast lookup, insertion, and deletion** using hashing.

Structure used in this problem:

```
Key   → number in array
Value → index of that number
```

Example map:

```
3 → 0
4 → 1
5 → 2
```

---

### Why `HashMap` is used

`HashMap` allows us to check if the **required complement already exists** in **constant time**.

Without a map:

- Brute force requires **two nested loops**
    
- Time complexity becomes **O(n²)**
    

With `HashMap`:

- Lookup is **O(1)**
    
- Overall complexity becomes **O(n)**
    

---

### How it is used in the solution

#### 1. Creating the Map

```java
HashMap<Integer, Integer> map = new HashMap<>();
```

Stores:

```
number → index
```

---

#### 2. Finding the Complement

```java
int complement = target - nums[i];
```

This calculates the value needed to reach the target.

---

#### 3. Checking if Complement Exists

```java
if (map.containsKey(complement))
```

If true:

- The complement was seen earlier
    
- The pair is found.
    

---

#### 4. Returning the Answer

```java
return new int[]{map.get(complement), i};
```

We return the stored index and the current index.

This automatically keeps the **smaller index first** because the earlier index was inserted first.

---

#### 5. Storing Elements

```java
map.put(nums[i], i);
```

Each element is stored for future complement checks.

---

## Complexity Analysis

### Time Complexity

```
O(n)
```

Each element is processed once.

HashMap operations (`put`, `get`, `containsKey`) are **O(1)** on average.

---

### Space Complexity

```
O(n)
```

In the worst case, all elements may be stored in the `HashMap`.

---

## Key Takeaways

- Transform the equation:
    

```
a + b = target
```

into

```
b = target - a
```

- Use `HashMap` to store **number → index**.
    
- Check complement before inserting current element.
    
- This reduces the problem from **O(n²) → O(n)**.
    
