---
type: concept
tags:
  - dsa/pattern
  - dsa/pattern/k-way-merge
difficulty: hard
pattern: "[[K-Way-Merge]]"
status: evergreen
related:
  - "[[Heap]]"
  - "[[Linked-List]]"
  - "[[Sorting]]"
  - "[[Divide-and-Conquer]]"
aliases:
  - K Way Merge
  - Merge k sorted
---


# K-Way Merge

> [!note] Definition
> Merge k sorted sequences (lists/arrays) into one sorted sequence. The canonical engine: a **min-heap** holding the current head of each sequence; repeatedly pop the smallest, append it, and push that sequence's next element. O(N log k) where N = total elements, k = number of sequences.

## When it applies (cues)
- "Merge k sorted linked lists / arrays."
- "K-th smallest element across k sorted arrays / in a sorted matrix."
- "Smallest range covering elements from k lists."
- "External sort" (merge sorted runs from disk).
- "Schedule the next smallest across many sources."
- "Find k closest across multiple sorted streams."

## Template — merge k sorted lists (heap of heads)
```java
ListNode mergeK(ListNode[] lists){
    PriorityQueue<ListNode> pq = new PriorityQueue<>((a,b) -> a.val - b.val);
    for (ListNode h : lists) if (h != null) pq.offer(h);     // one head per list
    ListNode dummy = new ListNode(0), cur = dummy;
    while (!pq.isEmpty()){
        ListNode n = pq.poll();                              // smallest current
        cur.next = n; cur = cur.next;
        if (n.next != null) pq.offer(n.next);                // refill from same list
    }
    return dummy.next;
}
```
> [!tip] The invariant: the heap always contains exactly one element per non-exhausted list — the next candidate from each. Pop the global min, then refill from that list.

## Template — k-th smallest in a sorted matrix / across arrays
```java
// matrix rows sorted; find k-th smallest
int kthSmallest(int[][] mat, int k){
    int n = mat.length;
    PriorityQueue<int[]> pq = new PriorityQueue<>((a,b) -> a[0] - b[0]);
    for (int i = 0; i < n; i++) pq.offer(new int[]{mat[i][0], i, 0});   // {val, row, col}
    int val = 0;
    while (k-- > 0){
        int[] t = pq.poll(); val = t[0]; int r = t[1], c = t[2];
        if (c + 1 < mat[r].length) pq.offer(new int[]{mat[r][c+1], r, c+1});
    }
    return val;
}
```

## Template — smallest range covering elements from k lists
Keep one pointer per list in a heap, track the current max; shrink the range `[min, max]` by advancing the list that held the min.
```java
PriorityQueue<int[]> pq = ...;   // {value, listIndex, elemIndex}
int max = ...; int[] best = {0, Integer.MAX_VALUE};
while (pq.size() == k){
    int[] min = pq.poll();
    if (max - min[0] < best[1] - best[0]) best = new int[]{min[0], max};
    // advance min's list; push next; update max
}
```

## Alternative — divide & conquer merge
Pair up the k lists and merge them two at a time, halving the count each round → O(N log k) too, no heap, O(log k) recursion depth.
```java
ListNode mergeKDC(ListNode[] lists){ return mergeRange(lists, 0, lists.length-1); }
ListNode mergeRange(ListNode[] L, int lo, int hi){
    if (lo > hi) return null; if (lo == hi) return L[lo];
    int mid = (lo+hi)/2;
    return mergeTwo(mergeRange(L, lo, mid), mergeRange(L, mid+1, hi));   // classic merge
}
```

## Complexity
- Heap approach: O(N log k) time, O(k) space.
- D&C approach: O(N log k) time, O(log k) recursion stack.
- Naive (concat + sort): O(N log N) — worse when k ≪ N.

> [!warning] Pitfalls
  - Comparator overflow: `(a,b) -> a.val - b.val` breaks for `Integer.MIN_VALUE`/`MAX_VALUE` extremes → use `Integer.compare(a.val, b.val)`.
  - Forgetting to refill the popped list's next element → that list silently drops out.
  - Heap should hold **at most one** element per list at any time (the current head), not all elements.
  - Comparator must match the desired order: ascending → natural `Integer.compare`; descending → reverse.
  - Ties: when values are equal, a stable secondary key (list index) avoids nondeterminism in "k-th" problems.

## Practice questions
- [[Merge-K-Sorted-Lists]] → heap or D&C
- [[Kth-Smallest-Element-in-a-Sorted-Matrix]] → heap of row heads
- [[Smallest-Range-Covering-Elements-from-K-Lists]] → sliding min/max over heap
- [[Find-K-Pairs-with-Smallest-Sums]] → heap over index pairs
- [[External-Sort]] (conceptual) → k-way merge of runs

## Related
- [[Heap]] · [[Linked-List]] · [[Sorting]] · [[Divide-and-Conquer]]
