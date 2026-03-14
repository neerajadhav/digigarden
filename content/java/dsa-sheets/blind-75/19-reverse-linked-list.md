---
title: 19 - Reverse Linked List
tags:
  - Linked-List
  - Easy
---

Given the beginning of a singly linked list `head`, reverse the list, and return the new beginning of the list.

---

## Examples

**Example 1**

Input: 
```
head = [0, 1, 2, 3]
```

Output: 
```
[3, 2, 1, 0]
```

---

**Example 2**

Input: 
```
head = []
```

Output: 
```
[]
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
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;

        while (curr != null) {
            // Save the next node
            ListNode nextTemp = curr.next;
            
            // Reverse the current node's pointer
            curr.next = prev;
            
            // Move pointers forward
            prev = curr;
            curr = nextTemp;
        }

        return prev;
    }
}
```

---

## Intuition

Reversing a Linked List iteratively involves re-orienting the pointers one by one without losing track of the rest of the list.

1. **Pointers**: We maintain three key pieces of information during each step:
   - `prev`: The node we just processed (which will become the `next` for the current node).
   - `curr`: The node we are currently "reversing".
   - `nextTemp`: A temporary reference to the *original* next node, so we don't lose the rest of the list when we rewrite `curr.next`.
2. **The Flip**: We set `curr.next = prev`.
3. **The Slide**: We move `prev` to `curr`, and `curr` to `nextTemp` to move to the next "link" in the chain.
4. **Conclusion**: When `curr` reaches `null`, the `prev` pointer is pointing at what used to be the tail, which is now the new head.

---

## Complexity Analysis

### Time Complexity

```
O(n)
```

We traverse the list exactly once.

---

### Space Complexity

```
O(1)
```

We only use two extra pointers (`prev` and `curr`), regardless of the list size.

---

## Key Takeaways

- **Iterative vs Recursive**: While recursion is elegant, the iterative approach is $O(1)$ space and avoids stack overflow for very long lists.
- **Three-Pointer Pattern**: Almost every "pointer manipulation" linked list problem involves holding onto a `next` temporary reference before breaking the current link.
- **Null Initialization**: Initializing `prev` to `null` handles the head becoming the new tail (pointing to nothing) perfectly.
