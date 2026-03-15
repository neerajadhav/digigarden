---
title: 23 - Remove Nth Node From End
tags:
  - Linked-List
  - Medium
---

## Problem Statement
Given the beginning of a linked list `head`, and an integer `n`. Remove the **nth node from the end** of the list and return the beginning of the list.

## Examples

### Example 1
**Input**: `head = [1,2,3,4], n = 2`  
**Output**: `[1,2,4]`  
**Explanation**: The 2nd node from the end is 3. After removing it, the list is [1,2,4].

### Example 2
**Input**: `head = [5], n = 1`  
**Output**: `[]`  
**Explanation**: There is only one node, and at $n=1$, it is removed.

### Example 3
**Input**: `head = [1,2], n = 2`  
**Output**: `[2]`  
**Explanation**: The 2nd node from the end is 1 (the head).

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
    public ListNode removeNthFromEnd(ListNode head, int n) {
        // Use a dummy node to handle edge cases like removing the head
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        
        ListNode first = dummy;
        ListNode second = dummy;
        
        // 1. Move 'first' pointer n steps ahead
        for (int i = 0; i <= n; i++) {
            first = first.next;
        }
        
        // 2. Move both pointers until 'first' reaches the end
        // This maintains a gap of n between 'first' and 'second'
        while (first != null) {
            first = first.next;
            second = second.next;
        }
        
        // 3. 'second' is now pointing to the node BEFORE the target node
        second.next = second.next.next;
        
        return dummy.next;
    }
}
```

## Intuition
The challenge is to identify the $n^{th}$ node from the end in a **single pass**.  
1. We use **two pointers** (`first` and `second`).
2. By moving the `first` pointer $n$ steps ahead, we create a gap of $n$ nodes between the two.
3. When `first` reaches the end of the list, `second` will be exactly at the node **preceding** the one to be removed.
4. Using a **dummy node** simplifies cases where the head needs to be deleted, as `second` will always have a valid `second.next`.

## Complexity Analysis
- **Time Complexity**: $O(sz)$, where $sz$ is the number of nodes in the list. We traverse the list once.
- **Space Complexity**: $O(1)$, as we only use two additional pointers regardless of the input size.

## Key Takeaways
- **Dummy Node Utility**: A placeholder node pointing to the head is a standard trick to handle deletions that might affect the head pointer itself.
- **One-Pass Constraint**: The two-pointer approach satisfies the optimal $O(n)$ requirement without needing to calculate the list's total length first.
- **Pointer Synchronization**: Maintaining a fixed distance between pointers is a powerful technique for finding relative positions from the end of a sequence.
