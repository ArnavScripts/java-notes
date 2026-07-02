---
type: question
tags:
  - dsa/ds/linked-list
  - dsa/pattern/fast-slow-pointers
  - fastslow
  - practice
  - practice/easy
difficulty: easy
pattern: "[[Fast-Slow-Pointers]]"
status: evergreen
source: "LeetCode 141"
related:
  - "[[Fast-Slow-Pointers]]"
  - "[[Linked-List]]"
  - "[[Linked-List-Cycle-II]]"
aliases:
  - Linked List Cycle
  - Has Cycle
---


# Linked List Cycle

> [!note] Problem
> Given `head`, determine if the linked list has a cycle (a node's `next` points back to an earlier node). Solve with O(1) space.

## Examples
```
Input:  3 -> 2 -> 0 -> -4
                  ^_____|    (tail connects to node index 1)
Output: true
```

## Brute force
- Store visited nodes in a `HashSet`; on revisit → cycle. O(n) time, **O(n) space** — fails the O(1) constraint.

## Optimal approach
- Pattern: [[Fast-Slow-Pointers]] (Floyd's tortoise & hare)
- Idea: slow moves 1, fast moves 2. If there's a cycle, fast laps slow and they meet; if no cycle, fast hits `null`.
- Steps:
  1. `slow = fast = head`.
  2. While `fast != null && fast.next != null`: `slow = slow.next; fast = fast.next.next`; if `slow == fast` return `true`.
  3. Return `false`.

## Complexity
- Time: O(n) · Space: O(1)

## Java solution
```java
public boolean hasCycle(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) return true;
    }
    return false;
}
```

> [!tip] Check `fast.next` **before** `fast.next.next` to avoid NPE on the last node. Both start at `head`; the first comparison happens after they've moved (so the `slow == fast` at `head` doesn't false-positive).

## Related
- [[Fast-Slow-Pointers]] · [[Linked-List]] · [[Linked-List-Cycle-II]] (find cycle start)
