---
title: 05 - Prefix Sum
tags:
  - Arrays
  - Range Query
  - Fundamentals
---

## Problem
Given an array `nums`, precompute information such that you can efficiently answer multiple "Range Sum Queries". Each query consists of two indices $L$ and $R$, and requires the sum of elements in `nums` from index $L$ to index $R$ inclusive.

## Intuition
The naive approach for each query is to iterate from $L$ to $R$ and calculate the sum, which takes $O(n)$ per query ($O(n \cdot q)$ total). Prefix sum optimizes this by precomputing a cumulative sum array $P$, where $P[i]$ stores the sum of all elements from index $0$ to $i$. Once $P$ is built, the sum of any range $[L, R]$ can be calculated in $O(1)$ time using the formula:
$$\text{Sum}(L, R) = P[R] - P[L-1]$$
*(Note: If $L=0$, the sum is simply $P[R]$).*

## Algorithm Steps
1. Create a `prefixSum` array of the same size as `nums`.
2. **Precomputation**:
    a. `prefixSum[0] = nums[0]`.
    b. For $i$ from $1$ to $n-1$: `prefixSum[i] = prefixSum[i-1] + nums[i]`.
3. **Query**:
    a. For a range $[L, R]$:
    b. If $L == 0$, return `prefixSum[R]`.
    c. Else, return `prefixSum[R] - prefixSum[L-1]`.

## Pseudocode
```text
procedure buildPrefixSum(A)
    P := new array of size length(A)
    P[0] := A[0]
    for i := 1 to length(A) - 1 do
        P[i] := P[i-1] + A[i]
    end for
    return P
end procedure

procedure queryRange(P, L, R)
    if L == 0 then
        return P[R]
    else
        return P[R] - P[L-1]
    end if
end procedure
```

## Java Code using JCF
Using a competitive programming structure.

```java
import java.util.*;

public class Main {
    /**
     * Builds the prefix sum array
     */
    public static long[] buildPrefixSum(List<Integer> nums) {
        int n = nums.size();
        long[] P = new long[n];
        if (n == 0) return P;
        
        P[0] = nums.get(0);
        for (int i = 1; i < n; i++) {
            P[i] = P[i - 1] + nums.get(i);
        }
        return P;
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
                    
                    long[] prefixSum = buildPrefixSum(nums);
                    
                    if (sc.hasNextInt()) {
                        int q = sc.nextInt(); // Number of queries
                        while (q-- > 0) {
                            if (sc.hasNextInt()) {
                                int L = sc.nextInt();
                                int R = sc.nextInt();
                                
                                long result = (L == 0) ? prefixSum[R] : prefixSum[R] - prefixSum[L - 1];
                                System.out.println(result);
                            }
                        }
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
6
1 3 5 7 9 11
2
0 2
2 5
```

**Output:**
```text
9
32
```

## Complexity
| Step | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **Precomputation** | $O(n)$ | $O(n)$ |
| **Query** | $O(1)$ | $O(1)$ |

- **Type**: Precomputation / Range Query.
- **Sorted Required**: No.
- **In-place**: Can be done in-place to save space.

## Example and Dry Run
**Input**: `nums = [1, 3, 5, 7, 9, 11]`, queries: `[0, 2]`, `[2, 5]`

1. **Precompute P**:
   - `P[0] = 1`
   - `P[1] = 1 + 3 = 4`
   - `P[2] = 4 + 5 = 9`
   - `P[3] = 9 + 7 = 16`
   - `P[4] = 16 + 9 = 25`
   - `P[5] = 25 + 11 = 36`
   - `P = [1, 4, 9, 16, 25, 36]`

2. **Query [0, 2]**: $L=0 \rightarrow P[2] = 9$.
3. **Query [2, 5]**: $L>0 \rightarrow P[5] - P[1] = 36 - 4 = 32$.

**Final Result**: 9, 32

## Variations
1. **2D Prefix Sum**: Efficiently find the sum of any rectangle in a matrix using $O(1)$ queries after $O(n \cdot m)$ precomputation.
2. **Difference Array**: Inverse of prefix sum; used to perform $O(1)$ range updates and then finding the final array values using prefix sum.
3. **Sum of Absolute Differences**: Using prefix sums to calculate sum of differences for each element relative to all others in $O(n)$.
