---
type: concept
tags:
  - dsa/ds
  - java/ds/linked-list
difficulty: medium
status: evergreen
related:
  - "[[Fast-Slow-Pointers]]"
  - "[[In-Place-Reversal]]"
  - "[[Stack]]"
  - "[[Queue]]"
  - "[[Inner-Classes]]"
aliases:
  - LinkedList
  - Singly Linked List
  - Doubly Linked List
---


# Linked List

> [!note] Definition
> A linked list stores nodes (`data` + `next` pointer), allocated non-contiguously. No random access → O(n) search, but **O(1)** insert/delete once you have the node reference. Variants: singly, doubly (prev+next), circular.

## Node & Java model
```java
static class ListNode {
    int val;
    ListNode next;            // singly
    ListNode(int x){ val = x; }
}
```
> [!tip] For doubly: add `ListNode prev;`. Java's `LinkedList` is doubly-linked and implements both `List` and `Deque` ([[Collections-Framework]]).

## Complexity
| Op | Time |
|----|------|
| access index i | O(n) |
| search value | O(n) |
| insert/delete at head | O(1) |
| insert/delete at known node | O(1) |
| insert/delete at tail (no tail ptr) | O(n) |

## The dummy-head trick
```java
ListNode dummy = new ListNode(0), cur = dummy;
// build or modify without special-casing the head
ListNode head = dummy.next;
```
Removes all "is head null?" branches — invaluable for merges, removals, reversals.

## Core manipulations
**Reverse** (iterative):
```java
ListNode prev = null, cur = head;
while (cur != null) { ListNode nxt = cur.next; cur.next = prev; prev = cur; cur = nxt; }
return prev;   // new head
```
> [!tip] This powers [[In-Place-Reversal]] (reverse whole, reverse in groups of k, reverse between positions).

**Merge two sorted**:
```java
ListNode d = new ListNode(0), c = d;
while (a != null && b != null) {
    if (a.val <= b.val) { c.next = a; a = a.next; } else { c.next = b; b = b.next; }
    c = c.next;
}
c.next = (a != null) ? a : b;
return d.next;
```
> [!tip] Foundation of merge sort on lists and [[K-Way-Merge]].

## Cycle detection (Floyd)
```java
ListNode slow = head, fast = head;
while (fast != null && fast.next != null) {
    slow = slow.next; fast = fast.next.next;
    if (slow == fast) { /* has cycle */ }
}
```
> [!tip] The [[Fast-Slow-Pointers]] pattern: cycle detection, find middle, find k-th from end.

## Find k-th from end
Move a leader k steps ahead, then walk both until leader hits null → follower is k-th from end. (Two pointers with offset.)

> [!tip] Pattern recognition cues
> - "Find middle / detect cycle / happy number" → [[Fast-Slow-Pointers]].
> - "Reverse / reverse in groups / reverse a portion" → [[In-Place-Reversal]].
> - "Merge k sorted lists / merge two sorted lists" → [[K-Way-Merge]].
> - "Palindromic linked list" → find middle + reverse second half + compare.
> - "Add two numbers represented as lists" → dummy head + carry.

> [!warning] Pitfalls
> - Losing the head pointer — keep `dummy.next` or return new head.
> - Modifying `next` before saving it → lost rest of list (`nxt = cur.next` first!).
> - Infinite loop if a cycle exists and you don't detect it.
> - Recursion on long lists → stack overflow ([[JVM-and-Memory]]); prefer iteration.

## Practice questions
- [[Reverse-Linked-List]] → [[In-Place-Reversal]]
- [[Linked-List-Cycle]] → [[Fast-Slow-Pointers]]
- [[Merge-Two-Sorted-Lists]] → [[K-Way-Merge]]
- [[Remove-Nth-Node-From-End]] → two pointers
- [[Palindrome-Linked-List]] → middle + reverse

## Related
- [[Fast-Slow-Pointers]] · [[In-Place-Reversal]] · [[K-Way-Merge]] · [[Stack]] · [[Queue]] · [[Inner-Classes]]
