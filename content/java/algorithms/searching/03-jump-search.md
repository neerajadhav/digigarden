---
title: 03 - Jump Search
tags:
  - Searching
  - Square-Root-Decomposition
---

# Jump Search

## Problem
Search for a target element in a **sorted** array by jumping ahead by fixed steps instead of searching element by element.

## Intuition
Linear Search is slow ($O(n)$) and Binary Search is fast ($O(\log n)$). Jump Search is an intermediate approach. Imagine you are looking for a specific house on a long street. instead of checking every house, you jump say 10 houses at a time. Once you pass the target house, you backtrack to the last checked house and do a linear search for the remaining 10 houses.

## Algorithm Steps
1. The optimal step size to jump is $m = \sqrt{n}$.
2. Jump ahead by $m$ until you find an element larger than the target or reach the end.
3. Once a larger element is found, perform a **Linear Search** from the previous jump position to the current position.
4. If found, return index; otherwise, return -1.

## Pseudocode
```text
procedure jumpSearch(A, target, n)
    step := sqrt(n)
    prev := 0
    while A[min(step, n) - 1] < target do
        prev := step
        step := step + sqrt(n)
        if prev >= n then return -1
    end while
    
    while A[prev] < target do
        prev := prev + 1
        if prev == min(step, n) then return -1
    end while
    
    if A[prev] == target then return prev
    return -1
end procedure
```

## Java Code using JCF
Using a competitive programming structure (TCS NQT Style).

```java
import java.util.*;

public class Main {
    /**
     * Jump Search Implementation
     */
    public static int jumpSearch(List<Integer> list, int target) {
        int n = list.size();
        int step = (int) Math.floor(Math.sqrt(n));
        int prev = 0;

        // Finding the block where element is present
        while (list.get(Math.min(step, n) - 1) < target) {
            prev = step;
            step += (int) Math.floor(Math.sqrt(n));
            if (prev >= n) return -1;
        }

        // Doing a linear search for target in block beginning with prev
        while (list.get(prev) < target) {
            prev++;
            // If we reached next block or end of list, target is not present
            if (prev == Math.min(step, n)) return -1;
        }

        if (list.get(prev) == target) return prev;
        return -1;
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
                    Collections.sort(list);
                    if (sc.hasNextInt()) {
                        int target = sc.nextInt();
                        System.out.println(jumpSearch(list, target));
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
1
9
0 1 1 2 3 5 8 13 21
5
```

**Output:**
```text
5
```

## Complexity
| Case | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **All Cases** | $O(\sqrt{n})$ | $O(1)$ |

- **Sorted Required**: **Yes**.
- **Comparison to others**: Faster than Linear Search, slower than Binary Search.
- **Advantage**: Better than Binary Search if "jumping back" is expensive in the system.

## Example and Dry Run
**Input**: `list = [0, 1, 1, 2, 3, 5, 8, 13, 21]`, `target = 5`, `n = 9`
- `step = sqrt(9) = 3`

1. `list[3-1] = list[2] = 1`. `1 < 5`. Jump! `prev=0, step=3`.
2. `list[6-1] = list[5] = 5`. `5 < 5` is false.
3. Linear search from `prev = 3` to `min(6, 9) = 6`.
   - `list[3] = 2`
   - `list[4] = 3`
   - `list[5] = 5`. **Found!** Return 5.

## Variations
1. **Block Search**: A general term for jump search with different block sizes.
2. **Two-Level Jump Search**: Searching in blocks of blocks.
