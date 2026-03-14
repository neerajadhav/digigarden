---
title: 07 - Counting Sort
tags:
  - Sorting
  - Non-Comparison-Based
---

# Counting Sort

## Problem
Sort an array of elements where the values are within a specific range (e.g., 0 to k). This is a non-comparison based sorting algorithm.

## Intuition
Counting Sort works by counting the occurrences of each unique element in the input. By calculating the cumulative counts, we can determine the exact position of each element in the final sorted array. It's like having a row of marked buckets; you count how many items go into each bucket and then place them back in order.

## Algorithm Steps
1. Find the maximum element in the input array to determine the range ($k$).
2. Create a `count` array of size $k + 1$ and initialize it with 0.
3. Iterate through the input array and increment `count[element]` for each element.
4. Update the `count` array by calculatingprefix sums: `count[i] = count[i] + count[i-1]`. This tells us the number of elements less than or equal to $i$.
5. Create an `output` array.
6. Iterate through the input array from **right to left** (to maintain stability):
   - Find the index in `count` array for current element.
   - Place current element at `output[count[element] - 1]`.
   - Decrement `count[element]`.
7. Copy the `output` array back to the original array.

## Pseudocode
```text
procedure countingSort(A, k)
    create count array C[0..k] and output array B[0..length(A)-1]
    initialize C with 0
    
    for x in A do
        C[x] := C[x] + 1
    
    for i := 1 to k do
        C[i] := C[i] + C[i-1]
        
    for i := length(A)-1 down to 0 do
        B[C[A[i]] - 1] := A[i]
        C[A[i]] := C[A[i]] - 1
        
    copy B to A
end procedure
```

## Java Code using JCF
Using a competitive programming structure (TCS NQT Style). This implementation handles positive integers.

```java
import java.util.*;

public class Main {
    /**
     * Stable Counting Sort Implementation
     */
    public static void countingSort(List<Integer> list) {
        if (list.isEmpty()) return;

        // 1. Find the maximum value to determine range
        int max = Collections.max(list);
        int min = Collections.min(list);
        
        // Handling range if there are negative numbers or large offsets
        int range = max - min + 1;
        int[] count = new int[range];
        int[] output = new int[list.size()];

        // 2. Count occurrences
        for (int num : list) {
            count[num - min]++;
        }

        // 3. Prefix sums
        for (int i = 1; i < range; i++) {
            count[i] += count[i - 1];
        }

        // 4. Build output array (right to left for stability)
        for (int i = list.size() - 1; i >= 0; i--) {
            int current = list.get(i);
            output[count[current - min] - 1] = current;
            count[current - min]--;
        }

        // 5. Copy back to list
        for (int i = 0; i < list.size(); i++) {
            list.set(i, output[i]);
        }
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
                    
                    countingSort(list);
                    
                    for (int i = 0; i < list.size(); i++) {
                        System.out.print(list.get(i) + (i == list.size() - 1 ? "" : " "));
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
7
1 4 1 2 7 5 2
5
10 3 10 2 1
```

**Output:**
```text
1 1 2 2 4 5 7
1 2 3 10 10
```

## Complexity
| Case | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **All Cases** | $O(n + k)$ | $O(n + k)$ |

- **$n$**: Number of elements in input array.
- **$k$**: Range of the input (Max - Min).
- **Stable**: Yes (if implemented correctly with right-to-left traversal).
- **In-place**: No ($O(n+k)$ extra space).
- **Note**: Best used when $k$ is not significantly larger than $n$.

## Example and Dry Run
**Input**: `[1, 4, 1, 2, 7, 5, 2]`  
**Range**: 1 to 7

1. **Count Array**:
   - `count[1]=2`, `count[2]=2`, `count[3]=0`, `count[4]=1`, `count[5]=1`, `count[6]=0`, `count[7]=1`
   - Initial: `[2, 2, 0, 1, 1, 0, 1]`

2. **Modified Count (Prefix Sums)**:
   - `[2, 4, 4, 5, 6, 6, 7]`

3. **Placing Elements (Right to Left)**:
   - Last element `2`: Pos = `count[2]` (4). Place `2` at index 3. `count[2]` becomes 3.
   - Next element `5`: Pos = `count[5]` (6). Place `5` at index 5. `count[5]` becomes 5.
   - ... and so on.

- **Result**: `[1, 1, 2, 2, 4, 5, 7]`

## Variations
1. **Handling Negative Numbers**: Subtract the minimum value from each element to map the range to non-negative indices (implemented in the Java code above).
2. **Radix Sort**: Uses Counting Sort as a subroutine to sort numbers digit by digit.
3. **Buckets**: If data is distributed across a large range but clustered, Bucket Sort might be more efficient.
