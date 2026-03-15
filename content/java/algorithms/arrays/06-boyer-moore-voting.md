---
title: 06 - Boyer-Moore Voting
tags:
  - Arrays
  - Fundamentals
---

## Problem
Given an array `nums` of size $n$, find the majority element. The majority element is the element that appears more than $\lfloor n/2 \rfloor$ times. You may assume that the majority element always exists in the array.

## Intuition
The Boyer-Moore Voting Algorithm is a stroke of genius for finding a majority element in $O(n)$ time and $O(1)$ space. Imagine a room full of people from different parties. If every person from one party "fights" and eliminates someone from a *different* party, the party that started with a majority will eventually be the only one left standing. 

In code, we maintain a `candidate` and a `count`. When the count is zero, we pick the current element as the new candidate. If we see the candidate again, we increment the count; otherwise, we decrement it.

## Algorithm Steps
1. Initialize `candidate = null` and `count = 0`.
2. Iterate through each element `x` in `nums`:
    a. If `count == 0`, set `candidate = x`.
    b. If `x == candidate`, increment `count`.
    c. Else, decrement `count`.
3. Return `candidate`.
4. *(Optional)* Perform a second pass to verify that the candidate actually appears $> n/2$ times if the existence is not guaranteed.

## Pseudocode
```text
procedure majorityElement(A)
    candidate := null
    count := 0
    for each x in A do
        if count == 0 then
            candidate := x
            count := 1
        else if x == candidate then
            count := count + 1
        else
            count := count - 1
        end if
    end for
    return candidate
end procedure
```

## Java Code using JCF
Using a competitive programming structure.

```java
import java.util.*;

public class Main {
    /**
     * Boyer-Moore Voting Algorithm to find majority element (> n/2)
     */
    public static int findMajority(List<Integer> nums) {
        int candidate = 0;
        int count = 0;

        for (int x : nums) {
            if (count == 0) {
                candidate = x;
                count = 1;
            } else if (x == candidate) {
                count++;
            } else {
                count--;
            }
        }
        
        // Optional: Verification pass (not needed if majority is guaranteed)
        /*
        int actualCount = 0;
        for (int x : nums) {
            if (x == candidate) actualCount++;
        }
        if (actualCount > nums.size() / 2) return candidate;
        return -1;
        */
        
        return candidate;
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
                    
                    int result = findMajority(nums);
                    System.out.println(result);
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
3
3 2 3
7
2 2 1 1 1 2 2
```

**Output:**
```text
3
2
```

## Complexity
| Case | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **All Cases** | $O(n)$ | $O(1)$ |

- **Type**: Iterative / Greedy.
- **In-place**: Yes.
- **Sorted Required**: No.

## Example and Dry Run
**Input**: `nums = [2, 2, 1, 1, 1, 2, 2]`

1. **x = 2**: `count = 0` $\rightarrow$ `candidate = 2`, `count = 1`.
2. **x = 2**: `x == candidate` $\rightarrow$ `count = 2`.
3. **x = 1**: `x != candidate` $\rightarrow$ `count = 1`.
4. **x = 1**: `x != candidate` $\rightarrow$ `count = 0`.
5. **x = 1**: `count = 0` $\rightarrow$ `candidate = 1`, `count = 1`.
6. **x = 2**: `x != candidate` $\rightarrow$ `count = 0`.
7. **x = 2**: `count = 0` $\rightarrow$ `candidate = 2`, `count = 1`.

**Final Result**: 2

## Variations
1. **Majority Element II**: Find all elements that appear more than $\lfloor n/3 \rfloor$ times. This uses two candidates and two counts.
2. **Majority Element (General)**: Find elements that appear more than $\lfloor n/k \rfloor$ times using $k-1$ candidates.
