---
title: 09 - Bucket Sort
tags:
  - Sorting
  - Distribution-Based
---

# Bucket Sort

## Problem
Sort an array by distributing elements into multiple buckets. Each bucket is then sorted individually, either using a different sorting algorithm or by recursively applying bucket sort.

## Intuition
Imagine you have a large pile of scattered mail. To sort it efficiently, you first put them into boxes based on the city (buckets). Then, you sort the mail within each city's box alphabetically. Finally, you combine the boxes in the order of the cities. Bucket Sort is most efficient when the input is uniformly distributed over a range.

## Algorithm Steps
1. Create $n$ empty buckets (usually lists).
2. For each element $x$ in the array, insert it into its corresponding bucket (e.g., index = $n \times x / \text{max\_val}$).
3. Sort each individual bucket using a stable sort (like Insertion Sort).
4. Concatenate all sorted buckets into the original array.

## Pseudocode
```text
procedure bucketSort(A, n)
    create n empty buckets B[0..n-1]
    for x in A do
        idx := index_of_bucket(x)
        insert x into B[idx]
    
    for i := 0 to n-1 do
        sort(B[i])
        
    concatenate buckets B[0]..B[n-1] into A
end procedure
```

## Java Code using JCF
Using a competitive programming structure (TCS NQT Style). This example assumes floating point numbers between 0 and 1, but is adaptable for integers.

```java
import java.util.*;

public class Main {
    /**
     * Bucket Sort Implementation
     */
    public static void bucketSort(List<Float> list) {
        int n = list.size();
        if (n <= 0) return;

        // 1. Create buckets
        List<List<Float>> buckets = new ArrayList<>(n);
        for (int i = 0; i < n; i++) {
            buckets.add(new ArrayList<>());
        }

        // 2. Put elements in buckets
        for (float val : list) {
            int bucketIdx = (int) (val * n);
            if (bucketIdx >= n) bucketIdx = n - 1; // Safeguard
            buckets.get(bucketIdx).add(val);
        }

        // 3. Sort individual buckets and concatenate
        int index = 0;
        for (List<Float> bucket : buckets) {
            Collections.sort(bucket);
            for (float val : bucket) {
                list.set(index++, val);
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
                    List<Float> list = new ArrayList<>();
                    for (int i = 0; i < n; i++) {
                        if (sc.hasNextFloat()) {
                            list.add(sc.nextFloat());
                        }
                    }
                    bucketSort(list);
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
1
5
0.897 0.565 0.656 0.1234 0.665
```

**Output:**
```text
0.1234 0.565 0.656 0.665 0.897
```

## Complexity
| Case | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| **Best/Average** | $O(n + k)$ | $O(n + k)$ |
| **Worst Case** | $O(n^2)$ | $O(n)$ |

- **Best/Average**: Occurs when elements are uniformly distributed.
- **Worst**: Occurs when all elements fall into the same bucket (becomes the complexity of the inner sort, e.g., $O(n \log n)$ or $O(n^2)$).
- **Stable**: Yes (if inner sort is stable).

## Variations
1. **Generic Bucket Sort**: Handling integers by mapping the range $[min, max]$ to $n$ buckets.
2. **Scatter-Gather Sort**: Parallel version of bucket sort.
3. **ProxmapSort**: A similar distribution-based sort using a "proximity map".
