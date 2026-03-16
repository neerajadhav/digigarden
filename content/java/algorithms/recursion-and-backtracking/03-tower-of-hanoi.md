---
title: 03 - Tower of Hanoi
tags:
  - Recursion
  - Classics
  - Optimization
---

# Tower of Hanoi (Generalized)

The Tower of Hanoi is a classic recursive puzzle. While the standard version uses 3 rods, the generalized version (often called the **Reve's Puzzle** for 4 rods) involves $k$ rods and $n$ disks.

## Problem
Move $n$ disks from a source rod to a destination rod using $k$ total rods, following these rules:
1. Only one disk can be moved at a time.
2. Each move consists of taking the upper disk from one of the stacks and placing it on top of another stack or on an empty rod.
3. No larger disk may be placed on top of a smaller disk.

## Intuition (3-Rod Case)
To move $n$ disks from **Source** to **Destination** using one **Auxiliary** rod:
1. Move $n-1$ disks from Source to Auxiliary.
2. Move the largest disk from Source to Destination.
3. Move $n-1$ disks from Auxiliary to Destination.

## Generalized Logic ($k$ Rods)
For $k > 3$, we use the **Frame-Stewart Algorithm**. The strategy is to choose an optimal number of disks $m$ to move to an intermediate rod first.

### Algorithm Steps
1. For $n$ disks and $k$ rods:
2. If $n=1$, move the disk directly.
3. If $k=3$, use the standard 3-rod recursive algorithm.
4. If $k>3$, choose a value $m$ ($1 \le m < n$):
    - Move $m$ disks to an auxiliary rod using all $k$ rods.
    - Move the remaining $n-m$ disks to the destination rod using $k-1$ rods (ignoring the rod currently holding $m$ disks).
    - Move the $m$ disks from the auxiliary rod to the destination rod using $k$ rods.

*Note: The choice of $m = n - \lfloor \sqrt{2n + 1} \rfloor + 1$ is often used as a heuristic for the 4-rod case.*

## Java Code using JCF

```java
import java.util.*;

public class Main {
    /**
     * Standard 3-Rod Tower of Hanoi
     */
    public static void towerOfHanoi3(int n, String src, String dest, String aux) {
        if (n == 0) return;
        if (n == 1) {
            System.out.println("Move disk 1 from " + src + " to " + dest);
            return;
        }
        towerOfHanoi3(n - 1, src, aux, dest);
        System.out.println("Move disk " + n + " from " + src + " to " + dest);
        towerOfHanoi3(n - 1, aux, dest, src);
    }

    /**
     * Generalized k-Rod Tower of Hanoi (Frame-Stewart)
     */
    public static void towerOfHanoiK(int n, int k, String src, String dest, List<String> auxList) {
        if (n == 0) return;
        if (n == 1) {
            System.out.println("Move disk 1 from " + src + " to " + dest);
            return;
        }
        
        if (k == 3) {
            towerOfHanoi3(n, src, dest, auxList.get(0));
            return;
        }

        // Heuristic for m (disks to move to auxiliary)
        int m = n / 2; // Simple heuristic for demonstration

        // 1. Move top m disks to the first auxiliary rod using k rods
        String firstAux = auxList.get(0);
        List<String> remainingAux = new ArrayList<>(auxList);
        remainingAux.remove(0);
        remainingAux.add(dest); // src is out, dest and other aux are available
        
        towerOfHanoiK(m, k, src, firstAux, remainingAux);

        // 2. Move remaining n-m disks to destination using k-1 rods
        List<String> subAuxList = new ArrayList<>(auxList);
        subAuxList.remove(0); // Cannot use the rod holding m disks
        towerOfHanoiK(n - m, k - 1, src, dest, subAuxList);

        // 3. Move m disks from first auxiliary to destination using k rods
        List<String> finalAuxList = new ArrayList<>(auxList);
        finalAuxList.remove(0);
        finalAuxList.add(src); // src is now empty
        towerOfHanoiK(m, k, firstAux, dest, finalAuxList);
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        if (sc.hasNextInt()) {
            int t = sc.nextInt();
            while (t-- > 0) {
                int n = sc.nextInt(); // Disks
                int k = sc.nextInt(); // Rods
                
                System.out.println("Solution for n=" + n + ", k=" + k + ":");
                List<String> aux = new ArrayList<>();
                for (int i = 1; i <= k - 2; i++) {
                    aux.add("Rod" + (i + 2));
                }
                towerOfHanoiK(n, k, "Rod1", "Rod2", aux);
                System.out.println();
            }
        }
        sc.close();
    }
}
```

## Sample Input and Output

### Input
```text
1
3 4
```

### Output
```text
Solution for n=3, k=4:
Move disk 1 from Rod1 to Rod3
Move disk 1 from Rod1 to Rod4
Move disk 2 from Rod1 to Rod2
Move disk 1 from Rod4 to Rod2
Move disk 1 from Rod3 to Rod2
```

## Complexity
- **3 Rods**: $O(2^n)$
- **4 Rods**: $O(2^{\sqrt{2n}})$ (Approximate efficiency of Frame-Stewart)

## Key Takeaways
- Adding more rods significantly reduces the number of moves required.
- The Frame-Stewart algorithm is the most recognized approach for $k > 3$, though it hasn't been strictly proven optimal for all $k$.
- Recursion depth and logic management become critical as the number of rods increases.
