---
title: 21 - Linked List Cycle Detection
tags:
  - Linked-List
  - Two-Pointers
  - Easy
---

Given the beginning of a linked list `head`, return `true` if there is a cycle in the linked list. Otherwise, return `false`.

There is a cycle in a linked list if at least one node in the list can be visited again by following the `next` pointer.

---

## Examples

**Example 1**

![[Pasted image 20260314140938.png]]

Input: 
```
head = [1, 2, 3, 4], index = 1
```

Output: 
```
true
```

Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).

---

**Example 2**

![[Pasted image 20260314140946.png]]

Input: 
```
head = [1, 2], index = -1
```

Output: 
```
false
```

---

## Solution

### Java Code

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
    public boolean hasCycle(ListNode head) {
        // Floyd's Cycle-Finding Algorithm (Fast and Slow pointers)
        ListNode slow = head;
        ListNode fast = head;

        while (fast != null && fast.next != null) {
            // Move slow pointer by 1 step
            slow = slow.next;
            // Move fast pointer by 2 steps
            fast = fast.next.next;

            // If they meet, there is a cycle
            if (slow == fast) {
                return true;
            }
        }

        // If fast reaches null, no cycle exists
        return false;
    }
}
```

---

## Intuition

Cycle detection in a linked list can be solved elegantly using **Floyd's Cycle-Finding Algorithm**, also known as the "Tortoise and Hare" algorithm.

1. **Two Pointers**: We use a `slow` pointer (moves 1 step at a time) and a `fast` pointer (moves 2 steps at a time).
2. **The Chase**:
   - If there is no cycle, the `fast` pointer will eventually reach the end of the list (`null`).
   - If there is a cycle, the `fast` pointer will eventually "lap" the `slow` pointer and they will meet at the same node.
3. **Why it works**: Inside a cycle, the distance between the fast and slow pointers increases by 1 in each step. Eventually, this distance will equal the length of the cycle, meaning the pointers will coincide.

---

## Complexity Analysis

### Time Complexity

```
O(n)
```

In the worst case, the fast pointer traverses the list and the cycle. The number of steps is proportional to the number of nodes.

---

### Space Complexity

```
O(1)
```

We only use two extra pointers (`slow` and `fast`), regardless of the list size.

---

## Key Takeaways

- **Tortoise and Hare** algorithm is the standard $O(1)$ space solution for cycle detection.
- Fast pointer must move **twice** as fast as the slow pointer.
- Always check `fast != null` and `fast.next != null` to avoid `NullPointerException`.
