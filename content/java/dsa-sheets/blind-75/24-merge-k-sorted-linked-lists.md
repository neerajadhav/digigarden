---
title: 24 - Merge K Sorted Linked Lists
tags:
  - Linked-List
  - Hard
---

## Problem Statement
You are given an array of $k$ linked lists `lists`, where each list is sorted in ascending order. Return the sorted linked list that is the result of merging all of the individual linked lists.

## Examples

### Example 1
**Input**: `lists = [[1,2,4],[1,3,5],[3,6]]`  
**Output**: `[1,1,2,3,3,4,5,6]`  
**Explanation**: Merging all lists into one sorted list.

### Example 2
**Input**: `lists = []`  
**Output**: `[]`

### Example 3
**Input**: `lists = [[]]`  
**Output**: `[]`

## Solution

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0) return null;
        
        // Min-Heap to store the current nodes of each list
        PriorityQueue<ListNode> pq = new PriorityQueue<>((a, b) -> a.val - b.val);
        
        // 1. Initial push: add the head of each non-empty list
        for (ListNode list : lists) {
            if (list != null) {
                pq.add(list);
            }
        }
        
        ListNode dummy = new ListNode(0);
        ListNode curr = dummy;
        
        // 2. Extract min and push next node
        while (!pq.isEmpty()) {
            ListNode node = pq.poll();
            curr.next = node;
            curr = curr.next;
            
            if (node.next != null) {
                pq.add(node.next);
            }
        }
        
        return dummy.next;
    }
}
```

## Intuition
To merge $k$ sorted lists, we need an efficient way to find the smallest element among $k$ candidates (one from each list) at every step.
1. A **Priority Queue (Min-Heap)** is the ideal data structure here. It keeps the smallest node at the top.
2. We start by adding the head of every list to the heap.
3. In each iteration, we extract the smallest node from the heap, append it to our result list, and add the *next* node from the same list into the heap.
4. This ensures that the heap always contains the "potential candidates" for the next smallest value in the global sorted sequence.

## Java Collections Framework Component Used
### PriorityQueue
- **What it is**: An unbounded priority queue based on a priority heap. The elements of the priority queue are ordered according to their natural ordering, or by a `Comparator`.
- **Why it is used**: It provides $O(\log k)$ time for both insertion and extraction of the minimum element. For $k$ lists, a simple scan for the minimum would take $O(k)$ per node, leading to $O(N \cdot k)$ complexity. The heap reduces this to $O(N \log k)$.
- **How it is used in the solution**: 
    - Initialized with a custom `Comparator` to compare `ListNode.val`.
    - `pq.add(node)`: Adds a node to the heap.
    - `pq.poll()`: Retrieves and removes the smallest node.

## Complexity Analysis
- **Time Complexity**: $O(N \log k)$, where $N$ is the total number of nodes across all lists and $k$ is the number of linked lists. Each of the $N$ nodes is added to and removed from the heap once.
- **Space Complexity**: $O(k)$, as the priority queue stores at most one node from each of the $k$ lists at any given time.

## Key Takeaways
- **Multiway Merge**: This problem is a classic example of merging multiple sorted streams, a concept also used in External Sorting.
- **Heap Utility**: Priority queues are indispensable when you need to frequently access the minimum or maximum element in a dynamic set of data.
- **Edge Case Handling**: Always check if the input array is empty or contains only null/empty lists to avoid null pointer exceptions.
 humanitarian
