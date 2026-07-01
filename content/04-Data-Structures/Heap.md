---
type: concept
tags:
  - java/ds/heap
difficulty: medium
pattern: ""
related:
  - "[[BST]]"
  - "[[K-Way-Merge]]"
  - "[[Sorting]]"
  - "[[Queue]]"
  - "[[Greedy]]"
aliases:
  - Heap
  - Priority Queue
  - Min Heap
  - Max Heap
---

# Heap / Priority Queue

> [!note] Definition
> A **heap** is a complete binary tree stored in an array where each parent compares correctly with its children: min-heap (parent ≤ children) or max-heap (parent ≥ children). The root is the min/max → O(1) peek, O(log n) insert/pop. Java's `PriorityQueue` is a min-heap by default.

## Array indexing (0-based)
- Left child: `2i + 1`
- Right child: `2i + 2`
- Parent: `(i - 1) / 2`

## Java API
```java
PriorityQueue<Integer> min = new PriorityQueue<>();
PriorityQueue<Integer> max = new PriorityQueue<>(Comparator.reverseOrder());
min.offer(3); min.offer(1); min.offer(2);
min.peek();   // 1 (root, smallest)
min.poll();   // 1  (removes root, re-heapifies)
min.size();
```
Custom comparator (e.g. by frequency, by distance):
```java
PriorityQueue<int[]> pq = new PriorityQueue<>((a,b) -> a[0]-b[0]);   // min by first
pq.offer(new int[]{dist, node});
```

## Complexity
| Op | Time |
|----|------|
| peek | O(1) |
| offer / poll | O(log n) |
| build from array (`heapify`) | O(n) |
| remove arbitrary | O(n) (no index access) |

## Classic uses
- **Top-k** (largest): min-heap of size k; evict the smallest when bigger arrives.
- **Top-k** (smallest): max-heap of size k, or full min-heap and poll k.
- **K-th largest / smallest**: heap of size k, or quickselect ([[Sorting]]).
- **Merge k sorted streams**: min-heap of heads → [[K-Way-Merge]].
- **Median of stream**: two heaps — left max-heap (lower half), right min-heap (upper half), keep sizes balanced.
- **Dijkstra / Prim**: min-heap keyed by distance/cost.

## Worked: top-k frequent elements
```java
List<Integer> topK(int[] a, int k) {
    Map<Integer,Integer> freq = new HashMap<>();
    for (int x : a) freq.merge(x, 1, Integer::sum);
    PriorityQueue<Integer> pq = new PriorityQueue<>((x,y) -> freq.get(x)-freq.get(y)); // min by freq
    for (int key : freq.keySet()) {
        pq.offer(key);
        if (pq.size() > k) pq.poll();    // keep only k most frequent
    }
    return new ArrayList<>(pq);
}
```

## Worked: median of data stream
```java
PriorityQueue<Integer> left  = new PriorityQueue<>(Comparator.reverseOrder()); // max
PriorityQueue<Integer> right = new PriorityQueue<>();                          // min
void addNum(int n){
    left.offer(n);
    right.offer(left.poll());
    if (left.size() < right.size()) left.offer(right.poll());
}
double median(){ return left.size() > right.size() ? left.peek() : (left.peek()+right.peek())/2.0; }
```

> [!tip] Pattern recognition cues
> - "Top-k", "k-th largest/smallest", "k closest" → heap of size k.
> - "Median / running median" → two heaps.
> - "Merge k sorted (lists/arrays)" → min-heap → [[K-Way-Merge]].
> - "Shortest path / MST" → min-heap as the priority structure.
> - "Schedule by next deadline / smallest processing time" → heap (often a [[Greedy]] move).

> [!warning] Pitfalls
  - `PriorityQueue` is **not** sorted iteration — iterating gives arbitrary order, only `poll`/`peek` are sorted.
  - Comparator sign confusion: `(a,b)->a-b` = ascending (min-heap). Beware integer overflow in `a-b`; use `Integer.compare(a,b)` for safety.
  - Heap of size k is O(n log k) — better than sorting all (O(n log n)) when k ≪ n.
  - To pop the k smallest you still pay O(k log n); quickselect is O(n) average for the k-th alone.

## Practice questions
- [[Kth-Largest-Element-in-an-Array]] → heap / quickselect
- [[Top-K-Frequent-Elements]] → freq map + heap
- [[Find-Median-from-Data-Stream]] → two heaps
- [[Merge-K-Sorted-Lists]] → [[K-Way-Merge]]
- [[K-Closest-Points-to-Origin]] → heap / quickselect

## Related
- [[BST]] (sorted alternatives) · [[K-Way-Merge]] · [[Sorting]] · [[Queue]] · [[Greedy]]
