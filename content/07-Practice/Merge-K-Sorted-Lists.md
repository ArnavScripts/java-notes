---
type: question
tags:
  - practice
  - hard
  - linkedlist
  - kwaymerge
  - heap
difficulty: hard
pattern: "[[K-Way-Merge]]"
source: "LeetCode 23"
related:
  - "[[K-Way-Merge]]"
  - "[[Heap]]"
  - "[[Merge-Two-Sorted-Lists]]"
  - "[[Divide-and-Conquer]]"
aliases:
  - Merge K Sorted Lists
---

# Merge k Sorted Lists

> [!note] Problem
> Given an array of `k` sorted linked-list heads, merge them into one sorted list and return its head.

## Examples
```
Input:  [1->4->5, 1->3->4, 2->6]
Output: 1->1->2->3->4->4->5->6
```

## Brute force
- Collect all values, sort, rebuild a list → O(N log N) time, O(N) space (N = total nodes).

## Optimal approach
- Pattern: [[K-Way-Merge]] (min-heap of heads)
- Idea: the smallest overall node is among the k current heads; keep a min-heap of one head per list, pop the min, advance that list.
- Steps:
  1. Push each non-null head into a `PriorityQueue` ordered by `val`.
  2. While the heap isn't empty: pop `n`, attach to the tail, push `n.next` if non-null.

## Complexity
- Time: O(N log k) — each of N nodes pushed/popped at log k cost.
- Space: O(k) for the heap.

## Java solution
```java
public ListNode mergeKLists(ListNode[] lists) {
    PriorityQueue<ListNode> pq = new PriorityQueue<>((a, b) -> Integer.compare(a.val, b.val));
    for (ListNode h : lists) if (h != null) pq.offer(h);
    ListNode dummy = new ListNode(0), cur = dummy;
    while (!pq.isEmpty()) {
        ListNode n = pq.poll();
        cur.next = n; cur = cur.next;
        if (n.next != null) pq.offer(n.next);          // refill from same list
    }
    return dummy.next;
}
```

> [!tip] Use `Integer.compare(a.val, b.val)` instead of `a.val - b.val` to be overflow-safe for `Integer.MIN_VALUE`/`MAX_VALUE` node values.

## Alternative: divide & conquer
Pair-merge the lists in rounds → O(N log k) time, O(log k) recursion stack. Same complexity, no heap.
```java
ListNode mergeRange(ListNode[] L, int lo, int hi) {
    if (lo > hi) return null; if (lo == hi) return L[lo];
    int mid = (lo + hi) / 2;
    return mergeTwo(mergeRange(L, lo, mid), mergeRange(L, mid + 1, hi));
}
```

## Related
- [[K-Way-Merge]] · [[Heap]] · [[Merge-Two-Sorted-Lists]] · [[Divide-and-Conquer]] · [[Kth-Smallest-Element-in-a-Sorted-Matrix]]
