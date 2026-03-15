---
title: 03 - Cycle Sort
tags:
  - Arrays
  - Sorting
  - Fundamentals
---

## Problem
Given an array containing $n$ objects, each with a unique value between $1$ and $n$, sort the array in $O(n)$ time using only constant extra space.

## Intuition
Cycle Sort (often called Cyclic Sort in competitive programming) is based on the idea that if we have a range of numbers from $1$ to $n$, each number $x$ should ideally be at index $x-1$. We iterate through the array, and for each element, if it's not at its correct position, we swap it with the element at its target position. We repeat this until the current index holds the correct element.

## Algorithm Steps
1. Start from the first index ($i = 0$).
2. While $i < n$:
    a. Determine the correct index for the current element: `correctIdx = nums[i] - 1`.
    b. If `nums[i]` is not at its correct position (`nums[i] != nums[correctIdx]`):
        - Swap `nums[i]` and `nums[correctIdx]`.
    c. Else (if it's already correct):
        - Move to the next index ($i++$).
3. The array is now sorted.

## Pseudocode
```text
procedure cyclicSort(A)
    i := 0
    while i < length(A) do
        correctIdx := A[i] - 1
        if A[i] != A[correctIdx] then
            swap(A[i], A[correctIdx])
        else
            i := i + 1
        end if
    end while
end procedure
```

## Java Code using JCF
Using a competitive programming structure.

```java
import java.util.*;

public class Main {
    /**
     * Cyclic Sort for numbers in range [1, n]
     */
    public static void cyclicSort(List<Integer> nums) {
        int i = 0;
        while (i < nums.size()) {
            int correctIdx = nums.get(i) - 1;
            // Handle only valid ranges to avoid IndexOutOfBounds
            if (nums.get(i) > 0 && nums.get(i) <= nums.size() && !nums.get(i).equals(nums.get(correctIdx))) {
                Collections.swap(nums, i, correctIdx);
            } else {
                i++;
            }
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        if (sc.hasNextInt()) {
            int t = sc.nextInt();
            while (t-- > 0) {
                if (sc.hasNextInt()) {
                    int n = sc.nextInt();
                    List<Integer> nums = new ArrayList<>();
                    for (int i = 0; i < n; i++) {
                        if (sc.hasNextInt()) {
                            nums.add(sc.nextInt());
                        }
                    }
                    
                    cyclicSort(nums);
                    
                    // Print sorted list
                    for (int i = 0; i < nums.size(); i++) {
                        System.out.print(nums.get(i) + (i == nums.size() - 1 ? "" : " "));
                    }
                    System.out.println();
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
3 5 2 1 4
6
1 2 4 3 6 5
```

**Output:**
```text
1 2 3 4 5
1 2 3 4 5 6
```

## Complexity
| Case | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **All Cases** | $O(n)$ | $O(1)$ |

- **Type**: Iterative / Swap-based.
- **In-place**: Yes.
- **Sorted Required**: No (It performs the sorting).
- **Stability**: Not Stable.

## Example and Dry Run
**Input**: `nums = [3, 5, 2, 1, 4]`

1. **i = 0, val = 3**: Correct index for 3 is $3-1 = 2$. Swap `nums[0]` and `nums[2]`.
   - Array: `[2, 5, 3, 1, 4]`
2. **i = 0, val = 2**: Correct index for 2 is $2-1 = 1$. Swap `nums[0]` and `nums[1]`.
   - Array: `[5, 2, 3, 1, 4]`
3. **i = 0, val = 5**: Correct index for 5 is $5-1 = 4$. Swap `nums[0]` and `nums[4]`.
   - Array: `[4, 2, 3, 1, 5]`
4. **i = 0, val = 4**: Correct index for 4 is $4-1 = 3$. Swap `nums[0]` and `nums[3]`.
   - Array: `[1, 2, 3, 4, 5]`
5. **i = 0, val = 1**: Correct index is 0. Already there. $i++$.
6. **i = 1, val = 2**: Correct. $i++$.
7. **i = 2, val = 3**: Correct. $i++$.
8. **i = 3, val = 4**: Correct. $i++$.
9. **i = 4, val = 5**: Correct. $i++$.

**Final Result**: `[1, 2, 3, 4, 5]`

## Variations
1. **Find Missing Number**: After sorting, the first index $i$ where `nums[i] != i + 1` is the missing number.
2. **Find All Duplicates**: If we encounter `nums[i] == nums[correctIdx]` during a swap attempt (and $i \neq correctIdx$), then `nums[i]` is a duplicate.
3. **First Missing Positive**: Standard cyclic sort application for unsorted arrays containing any integers.
