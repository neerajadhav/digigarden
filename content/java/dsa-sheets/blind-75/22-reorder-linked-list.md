---
title: 22 - Reorder Linked List
tags:
  - Linked-List
  - Medium
---

## Problem Statement
You are given the head of a singly linked-list. Reorder the nodes of the linked list to be in the following order:
$L_0 \rightarrow L_n \rightarrow L_1 \rightarrow L_{n-1} \rightarrow L_2 \rightarrow L_{n-2} \rightarrow \dots$

You may not modify the values in the list's nodes; only nodes themselves may be changed.

## Examples

### Example 1
**Input**: `head = [2,4,6,8]`  
**Output**: `[2,8,4,6]`  
**Explanation**: The nodes are reordered to follow the specified sequence.

### Example 2
**Input**: `head = [2,4,6,8,10]`  
**Output**: `[2,10,4,8,6]`  
**Explanation**: For odd lengths, the middle node remains at the end.

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
    public void reorderList(ListNode head) {
        if (head == null || head.next == null) return;

        // 1. Find the middle of the linked list
        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }

        // 2. Reverse the second half of the list
        ListNode second = reverse(slow.next);
        slow.next = null; // Break the link

        // 3. Merge the two halves
        ListNode first = head;
        while (second != null) {
            ListNode temp1 = first.next;
            ListNode temp2 = second.next;

            first.next = second;
            second.next = temp1;

            first = temp1;
            second = temp2;
        }
    }

    private ListNode reverse(ListNode head) {
        ListNode prev = null, curr = head;
        while (curr != null) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }
}
```

## Intuition
The problem asks for a specific "weaving" of the first half and the reversed second half of the list.
1. To find the starting point of the second half, we use the **Fast and Slow Pointer** technique to find the middle.
2. Since we need nodes from the end in reverse order ($L_n, L_{n-1} \dots$), we **reverse the second half** of the list.
3. Finally, we **merge** the two lists by alternating nodes from each.

## Complexity Analysis
- **Time Complexity**: $O(n)$, where $n$ is the number of nodes. We traverse the list once to find the middle, once to reverse the half, and once to merge.
- **Space Complexity**: $O(1)$, as we only use a few pointers and perform the reordering in-place.

## Key Takeaways
- **Efficiency**: Solving the problem in-place avoids $O(n)$ space complexity that would be required if we stored the nodes in a list or stack.
- **Modular Approach**: Dividing the problem into three sub-tasks (find middle, reverse, merge) makes the implementation cleaner and less error-prone.
- **Pointer Manipulation**: Breaking the link (`slow.next = null`) is crucial to treat the first half as a separate terminated list.
