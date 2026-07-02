---
type: concept
tags:
  - dsa/ds
  - java/ds/queue
difficulty: easy
status: evergreen
related:
  - "[[Stack]]"
  - "[[Graph-BFS]]"
  - "[[Heap]]"
  - "[[Linked-List]]"
  - "[[Tree-BFS]]"
aliases:
  - Queue DS
  - Deque
  - FIFO
---


# Queue

> [!note] Definition
> A **FIFO** container: first-in, first-out. Enqueue at back, dequeue from front, both O(1). A **deque** allows both ends. Models BFS frontiers, scheduling, sliding-window state.

## Java choices
```java
Queue<Integer> q = new ArrayDeque<>();   // plain FIFO
q.offer(1); q.poll(); q.peek();          // returns null on empty (no exception)

Deque<Integer> dq = new ArrayDeque<>();  // both ends
dq.offerFirst(x); dq.offerLast(x);
dq.pollFirst();  dq.pollLast();
dq.peekFirst();  dq.peekLast();
```
- `ArrayDeque` → fast array-backed deque (preferred for stack **and** queue).
- `LinkedList` → also a `Deque` (doubly linked) but more overhead per node.
- `PriorityQueue` → a **heap**, not FIFO — see [[Heap]].

## Complexity
`ArrayDeque`: all end ops O(1) amortized. `PriorityQueue`: offer/poll O(log n).

## Classic uses
- **BFS frontier**: `offer` neighbors, `poll` current. See [[Graph-BFS]], [[Tree-BFS]].
- **Level-order traversal** of a tree.
- **Sliding window maximum**: a deque holding useful indices. See [[Sliding-Window]] / [[Monotonic-Stack]].
- **Task scheduling / round-robin**.
- **Producer-consumer** → `BlockingQueue` ([[Multithreading]]).

## Worked: BFS skeleton
```java
Queue<Node> q = new ArrayDeque<>();
q.offer(start);
while (!q.isEmpty()) {
    Node cur = q.poll();
    if (done(cur)) return;
    for (Node nb : neighbors(cur)) q.offer(nb);
}
```

## Worked: monotonic deque for sliding-window max
```java
int[] maxSliding(int[] a, int k) {
    int n = a.length; int[] res = new int[n-k+1];
    Deque<Integer> dq = new ArrayDeque<>();      // indices, values decreasing
    for (int i = 0; i < n; i++) {
        while (!dq.isEmpty() && dq.peekFirst() <= i-k) dq.pollFirst();   // out of window
        while (!dq.isEmpty() && a[dq.peekLast()] <= a[i]) dq.pollLast(); // useless
        dq.offerLast(i);
        if (i >= k-1) res[i-k+1] = a[dq.peekFirst()];
    }
    return res;
}
```

> [!tip] Pattern recognition cues
> - "Shortest path / fewest steps on unweighted graph or grid" → BFS with a queue → [[Graph-BFS]].
> - "Process level by level" → queue → [[Tree-BFS]].
> - "Max/min over every sliding window of size k" → monotonic deque.
> - "Top-k / priority" → [[Heap]], not a plain queue.

> [!warning] Pitfalls
> - `add`/`remove` throw on capacity/empty; `offer`/`poll` return special values — prefer the latter.
> - Forgetting to mark visited on **enqueue** (not dequeue) → duplicates and exponential blowup in BFS.
> - `PriorityQueue` is **not** stable and offers no O(1) `peekLast`; if you need both ends sorted, consider a balanced BST (`TreeSet`).

## Practice questions
- [[Number-of-Islands]] → [[Graph-BFS]] (also DFS/Union-Find)
- [[Binary-Tree-Level-Order-Traversal]] → [[Tree-BFS]]
- [[Sliding-Window-Maximum]] → monotonic deque
- [[Implement-Queue-using-Stacks]] → two stacks

## Related
- [[Stack]] · [[Graph-BFS]] · [[Tree-BFS]] · [[Heap]] · [[Linked-List]] · [[Sliding-Window]]
