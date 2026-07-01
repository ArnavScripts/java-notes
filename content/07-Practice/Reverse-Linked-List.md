---
type: question
tags:
  - practice
  - easy
  - linkedlist
  - inplacereversal
difficulty: easy
pattern: "[[In-Place-Reversal]]"
source: "LeetCode 206"
related:
  - "[[In-Place-Reversal]]"
  - "[[Linked-List]]"
  - "[[Reverse-Linked-List-II]]"
aliases:
  - Reverse Linked List
---

# Reverse Linked List

> [!note] Problem
> Given the `head` of a singly linked list, reverse it and return the new head.

## Examples
```
Input:  1 -> 2 -> 3 -> 4 -> 5
Output: 5 -> 4 -> 3 -> 2 -> 1
```

## Brute force
- Copy values into an array, reverse, write back → O(n) time, O(n) space. Not the point.

## Optimal approach
- Pattern: [[In-Place-Reversal]]
- Idea: iterate with three pointers (`prev`, `cur`, `next`); flip each `cur.next` to `prev`.
- Steps:
  1. `prev = null, cur = head`.
  2. While `cur != null`: save `next = cur.next`; set `cur.next = prev`; advance `prev = cur; cur = next`.
  3. Return `prev` (new head).

## Complexity
- Time: O(n) · Space: O(1)

## Java solution
```java
public ListNode reverseList(ListNode head) {
    ListNode prev = null, cur = head;
    while (cur != null) {
        ListNode next = cur.next;   // save rest
        cur.next = prev;            // flip
        prev = cur; cur = next;     // advance
    }
    return prev;
}
```

> [!tip] Save `next` **before** overwriting `cur.next`, or you lose the tail. This one-liner order is the most common bug.

## Recursive variant
```java
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode newHead = reverseList(head.next);
    head.next.next = head;     // the node after head now points back
    head.next = null;
    return newHead;
}
```
> [!warning] Recursion uses O(n) stack — risk overflow on long lists ([[JVM-and-Memory]]). Prefer iterative.

## Related
- [[In-Place-Reversal]] · [[Linked-List]] · [[Reverse-Linked-List-II]] · [[Reverse-Nodes-in-k-Group]]
