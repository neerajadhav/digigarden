---
title: 05 - Top K Frequent Elements
tags:
  - Array
  - HashMap
  - Heap
  - PriorityQueue
  - Hashing
  - Medium
---

Given an integer array `nums` and an integer `k`, return the `k` most frequent elements in the array.

The test cases are generated such that the answer is always unique.

You may return the output in any order.

## Examples

Example 1

Input
```

nums = [1,2,2,3,3,3]  
k = 2

```

Output
```

[2,3]

```

Example 2

Input
```

nums = [7,7]  
k = 1

```

Output
```

[7]

````

## Solution

```java
import java.util.*;

class Solution {
    public int[] topKFrequent(int[] nums, int k) {

        HashMap<Integer, Integer> map = new HashMap<>();

        for (int num : nums) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }

        PriorityQueue<Integer> pq = new PriorityQueue<>(
            (a, b) -> map.get(b) - map.get(a)
        );

        pq.addAll(map.keySet());

        int[] result = new int[k];

        for (int i = 0; i < k; i++) {
            result[i] = pq.poll();
        }

        return result;
    }
}
````

## Intuition

The problem asks for the elements that appear most frequently.

Steps:

1. Count the frequency of every number.
    
2. Store these frequencies in a structure that can quickly give the highest frequency element.
    
3. Extract the top `k` elements.
    

Example

```
nums = [1,2,2,3,3,3]
```

Frequency map

```
1 -> 1
2 -> 2
3 -> 3
```

The most frequent elements are:

```
3 (3 times)
2 (2 times)
```

Return

```
[3,2]
```

Order does not matter.

## Java Collections Framework Components Used

### HashMap

Used to store frequency of each number.

Structure

```
Key   -> number
Value -> frequency
```

Example

```
1 -> 1
2 -> 2
3 -> 3
```

Method used

`getOrDefault(key, defaultValue)`

Used to simplify frequency counting.

Example

```
map.put(num, map.getOrDefault(num, 0) + 1);
```

### PriorityQueue (Max Heap)

Used to retrieve elements with the highest frequency.

Comparator used

```
(a, b) -> map.get(b) - map.get(a)
```

This ensures the element with the **highest frequency comes first**.

Important methods

`addAll(collection)`  
Adds all keys into the heap.

`poll()`  
Removes and returns the element with highest priority.

## Complexity Analysis

Let

```
n = number of elements
```

Time Complexity

```
O(n log n)
```

- Building the frequency map takes `O(n)`
    
- Heap operations take `O(log n)`
    

Space Complexity

```
O(n)
```

Used by the hashmap and priority queue.

## Key Takeaways

- First count frequencies using `HashMap`.
    
- Use `PriorityQueue` to retrieve highest frequency elements.
    
- This combination of `HashMap + Heap` is a common interview pattern.