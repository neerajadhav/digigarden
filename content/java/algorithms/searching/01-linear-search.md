---
title: 01 - Linear Search
tags:
  - Searching
  - Fundamentals
---

# Linear Search

## Problem
Given an array (or list) and a target element, find the index of the target element in the array. If the element is not present, return -1.

## Intuition
Linear search is the simplest searching algorithm. Imagine looking for a specific book on a shelf; you start from one end and check every book one by one until you find the one you want or reach the other end. It doesn't require the data to be sorted.

## Algorithm Steps
1. Start from the first element (index 0).
2. Compare the current element with the target.
3. If the current element matches the target, return the current index.
4. If it doesn't match, move to the next element.
5. Repeat steps 2-4 until the end of the array is reached.
6. If the end is reached and the target is not found, return -1.

## Pseudocode
```text
procedure linearSearch(A, target)
    for i := 0 to length(A) - 1 do
        if A[i] == target then
            return i
        end if
    end for
    return -1
end procedure
```

## Java Code using JCF
Using a competitive programming structure (TCS NQT Style).

```java
import java.util.*;

public class Main {
    /**
     * Linear Search on a List
     */
    public static int linearSearch(List<Integer> list, int target) {
        for (int i = 0; i < list.size(); i++) {
            if (list.get(i) == target) {
                return i; // Target found
            }
        }
        return -1; // Target not found
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        if (sc.hasNextInt()) {
            int t = sc.nextInt();
            while (t-- > 0) {
                if (sc.hasNextInt()) {
                    int n = sc.nextInt();
                    List<Integer> list = new ArrayList<>();
                    for (int i = 0; i < n; i++) {
                        if (sc.hasNextInt()) {
                            list.add(sc.nextInt());
                        }
                    }
                    
                    if (sc.hasNextInt()) {
                        int target = sc.nextInt();
                        int result = linearSearch(list, target);
                        System.out.println(result);
                    }
                }
            }
        }
        sc.close();
    }
}
```

### Sample Input and Output

**Input:**
```text
2
5
10 20 30 40 50
30
4
1 3 5 7
2
```

**Output:**
```text
2
-1
```

## Complexity
| Case | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **Best Case** | $O(1)$ (Target is at the first index) | $O(1)$ |
| **Average Case** | $O(n)$ | $O(1)$ |
| **Worst Case** | $O(n)$ (Target is at the last index or not present) | $O(1)$ |

- **Type**: Iterative / Sequential.
- **Sorted Required**: No.

## Example and Dry Run
**Input**: `list = [10, 20, 30, 40, 50]`, `target = 30`

1. **index 0**: `list[0] = 10`. `10 != 30`. Continue.
2. **index 1**: `list[1] = 20`. `20 != 30`. Continue.
3. **index 2**: `list[2] = 30`. `30 == 30`. **Found!** Return 2.

**Input**: `list = [1, 3, 5, 7]`, `target = 2`

1. **index 0**: `1 != 2`
2. **index 1**: `3 != 2`
3. **index 2**: `5 != 2`
4. **index 3**: `7 != 2`
5. End of list reached. Return -1.

## Variations
1. **Global Linear Search**: Instead of returning the first occurrence, return all indices where the target is found.
2. **Recursive Linear Search**: Implement the same logic using recursion.
3. **Search in String**: Finding a character in a string follows the same linear search principle.
