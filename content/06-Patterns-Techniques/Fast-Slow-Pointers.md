---
type: concept
tags:
  - dsa/pattern/fast-slow
difficulty: medium
pattern: "[[Fast-Slow-Pointers]]"
related:
  - "[[Linked-List]]"
  - "[[Two-Pointers]]"
  - "[[Math]]"
aliases:
  - Fast Slow Pointers
  - Floyd's algorithm
  - Tortoise and Hare
---

# Fast & Slow Pointers

> [!note] Definition
> Two pointers moving at **different speeds** (slow = 1 step, fast = 2 steps) through a linked structure. If there's a cycle, fast laps slow and they meet; if not, fast reaches the end. Also finds the middle in one pass.

## When it applies (cues)
- "Detect a cycle in a linked list."
- "Find the start of the cycle."
- "Find the middle of a linked list."
- "Happy number" (a number sequence either loops or hits 1) — treat as a cycle-detection on an implicit graph.
- "Find duplicate in an array of 1..n" (treat array values as pointers → implicit cycle).

## Template — cycle detection
```java
boolean hasCycle(ListNode head){
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null){
        slow = slow.next; fast = fast.next.next;
        if (slow == fast) return true;     // met => cycle
    }
    return false;                          // fast reached end
}
```

## Template — find cycle start (Floyd's full)
After they meet, reset one pointer to `head` and move **both at the same slow speed**; they meet at the cycle start.
```java
ListNode cycleStart(ListNode head){
    ListNode slow = head, fast = head, meet = null;
    while (fast != null && fast.next != null){
        slow = slow.next; fast = fast.next.next;
        if (slow == fast){ meet = slow; break; }
    }
    if (meet == null) return null;             // no cycle
    ListNode p = head;
    while (p != meet){ p = p.next; meet = meet.next; }
    return p;                                  // cycle entry
}
```
**Why**: if the cycle starts after `d` nodes and has length `c`, the meeting point is `c − (d mod c)` ahead; moving both from head and from meet at the same speed converges at the entry.

## Template — middle of list
```java
ListNode mid(ListNode head){
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null){ slow = slow.next; fast = fast.next.next; }
    return slow;     // for even length this is the 2nd middle; use fast=head.next for 1st
}
```

## Worked: happy number
A number is happy if repeatedly replacing it by the sum of squares of its digits reaches 1. The process either hits 1 or loops → cycle detection on the implicit sequence.
```java
int sqDigitSum(int n){ int s=0; while(n>0){ int d=n%10; s+=d*d; n/=10; } return s; }
boolean isHappy(int n){
    int slow = n, fast = n;
    do { slow = sqDigitSum(slow); fast = sqDigitSum(sqDigitSum(fast)); }
    while (slow != fast);
    return slow == 1;
}
```

## Worked: find duplicate (Floyd on array-as-graph)
Given array `a[1..n]` with values in `1..n` and exactly one duplicate: interpret `i → a[i]` as pointers; the duplicate is the cycle entry.
```java
int findDuplicate(int[] a){
    int slow = a[0], fast = a[0];
    do { slow = a[slow]; fast = a[a[fast]]; } while (slow != fast);
    int p = a[0];
    while (p != slow){ p = a[p]; slow = a[slow]; }
    return p;
}
```
> [!tip] O(n) time, O(1) space — beats the [[HashMap]] counter and the sort approaches when you must not modify the array.

## Complexity
- Time: O(n) — meeting happens within one lap.
- Space: O(1) — the big win over a visited-set.

> [!warning] Pitfalls
  - `fast.next` must be checked **before** `fast.next.next` to avoid NPE.
  - "Middle" for even-length lists has two middles — pick the convention your problem wants (1st or 2nd) and adjust the start (`fast = head` vs `fast = head.next`).
  - For the array-as-graph duplicate trick, the array must satisfy `1..n` values in `1..n` slots; otherwise the pointer interpretation breaks.
  - `do/while` (not `while`) for the duplicate case so the loop runs at least once before comparing equal start positions.

## Practice questions
- [[Linked-List-Cycle]] → detect
- [[Linked-List-Cycle-II]] → find start
- [[Middle-of-the-Linked-List]] → middle
- [[Happy-Number]] → implicit cycle
- [[Find-the-Duplicate-Number]] → array-as-graph
- [[Palindrome-Linked-List]] → middle + reverse half

## Related
- [[Linked-List]] · [[Two-Pointers]] · [[Math]]
