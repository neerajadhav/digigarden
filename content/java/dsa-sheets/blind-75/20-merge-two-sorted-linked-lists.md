---
title: 20 - Merge Two Sorted Linked Lists
tags:
  - Linked-List
  - Easy
---

You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists into one **sorted** linked list and return the head of the new sorted linked list.

The new list should be made up of nodes from `list1` and `list2`.

---

## Examples

**Example 1**

![Merge Two Sorted Lists](../../../../assets/attachments/Pasted%20image%2020260314140757.png)

Input: 
```
list1 = [1, 2, 4], list2 = [1, 3, 5]
```

Output: 
```
[1, 1, 2, 3, 4, 5]
```

---

**Example 2**

Input: 
```
list1 = [], list2 = [1, 2]
```

Output: 
```
[1, 2]
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
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        // Dummy node to simplify the head handling
        ListNode dummy = new ListNode(0);
        ListNode curr = dummy;

        while (list1 != null && list2 != null) {
            if (list1.val <= list2.val) {
                curr.next = list1;
                list1 = list1.next;
            } else {
                curr.next = list2;
                list2 = list2.next;
            }
            curr = curr.next;
        }

        // Attach the remaining part of the list that isn't null
        if (list1 != null) {
            curr.next = list1;
        } else {
            curr.next = list2;
        }

        return dummy.next;
    }
}
```

---

## Intuition

Merging two sorted lists is similar to the "merge" step in Merge Sort. We compare the heads of both lists and pick the smaller one to append to our new list.

1. **Dummy Node**: Creating a `dummy` node at the start is a common technique in Linked List problems. It avoids the need for special `if` checks to handle the `head` of the resulting list.
2. **Comparison Loop**: We iterate until one of the lists becomes empty. At each step, we connect the smaller node to our "merged" chain.
3. **Remainder**: Once the loop breaks, one of the lists might still have nodes (since they may be of different lengths or one had larger values at the end). Since the input lists are already sorted, we simply attach the remainder to the end of our merged list.

---

## Complexity Analysis

### Time Complexity

```
O(n + m)
```

Where `n` and `m` are the lengths of the two lists. We traverse each node exactly once.

---

### Space Complexity

```
O(1)
```

We are not creating new nodes (except for the dummy node); we are simply re-pointing the existing `next` pointers.

---

## Key Takeaways

- **Dummy Nodes** are highly effective for simplifying list initialization and edge cases.
- **Reference Re-use**: Merging is an in-place operation that changes existing node references.
- Always check for **remaining nodes** after the main loop finishes.
