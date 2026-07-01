---
type: concept
tags:
  - dsa/algo/sorting
difficulty: medium
pattern: ""
related:
  - "[[Arrays]]"
  - "[[Divide-and-Conquer]]"
  - "[[Heap]]"
  - "[[Two-Pointers]]"
  - "[[Merge-Intervals]]"
aliases:
  - Sorting
  - Quicksort
  - Mergesort
  - Counting sort
---

# Sorting

> [!note] Definition
> Reorder elements into a total order. Comparison sorts are Ω(n log n) lower bound; non-comparison sorts (counting/radix) beat it with bounded keys. Sorting unlocks [[Two-Pointers]], [[Merge-Intervals]], binary search, and greedy proofs.

## Java sorts
```java
int[] a = ...;
Arrays.sort(a);                       // primitives: dual-pivot quicksort, O(n log n) avg
Arrays.sort(arr, (x,y)->x[0]-y[0]);   // objects: TimSort (stable), O(n log n)
List<Integer> list = ...;
Collections.sort(list);               // stable, TimSort
list.sort(Comparator.comparingInt(Node::w).reversed().thenComparing(Node::id));
```

## Comparison sorts (know the trade-offs)
| Sort | Avg | Worst | Stable? | Notes |
|------|-----|-------|:---:|-------|
| Quicksort | O(n log n) | O(n²) | no | in-place; pivot choice matters |
| Mergesort | O(n log n) | O(n log n) | yes | O(n) extra space; stable; good for lists |
| Heapsort | O(n log n) | O(n log n) | no | in-place; worse cache behavior |
| TimSort | O(n log n) | O(n log n) | yes | adaptive; used by Java for objects |

## Quicksort skeleton (Lomuto partition)
```java
void qs(int[] a, int lo, int hi){
    if (lo >= hi) return;
    int p = a[hi], i = lo;
    for (int j = lo; j < hi; j++) if (a[j] < p) swap(a, i++, j);
    swap(a, i, hi);
    qs(a, lo, i-1); qs(a, i+1, hi);
}
```
> [!warning] Worst case O(n²) on sorted input with naive pivot → randomize or use median-of-three.

## Mergesort skeleton
```java
void ms(int[] a, int lo, int hi){
    if (lo >= hi) return;
    int mid = (lo+hi)/2;
    ms(a, lo, mid); ms(a, mid+1, hi);
    merge(a, lo, mid, hi);   // merge two sorted halves into a temp
}
```
Stable and O(n log n) guaranteed; basis for counting inversions and [[Merge-Intervals]].

## Non-comparison sorts
- **Counting sort** — O(n + k) when keys in [0, k). Great for small ranges (ages 0–120, ranks 1–n).
- **Radix sort** — O(d·(n + b)) digit-by-digit (LSD first); good for fixed-width integers/strings.
- **Bucket sort** — distribute into buckets, sort each; good for uniform floats.

## Comparators (the practical core)
```java
Comparator<Point> byX = (a,b) -> Integer.compare(a.x, b.x);  // overflow-safe
Comparator<Point> byXThenY = Comparator.comparingInt((Point p)->p.x).thenComparingInt(p->p.y);
Arrays.sort(intervals, (a,b) -> a[0] - b[0]);                // sort intervals by start
```
> [!tip] Use `Integer.compare(a,b)` instead of `a - b` to avoid integer overflow on large values.

## Selection algorithms
- **Quickselect** — average O(n) to find the k-th smallest (partition and recurse into the side containing k). Used for [[Kth-Largest-Element-in-an-Array]].
- **Median-of-medians** — worst-case O(n) (theoretical; rarely needed).

> [!tip] Pattern recognition cues
> - "Sort then two-pointer / merge / count inversions" → sort first.
> - "Intervals overlap / merge / insert" → sort by start → [[Merge-Intervals]].
> - "Top-k / k-th" → [[Heap]] or quickselect (sort works but is O(n log n)).
> - "Small-range keys (1..n, counts)" → counting sort, O(n).
> - "Custom ordering / multi-key" → comparator chain.

> [!warning] Pitfalls
  - `Arrays.sort` on primitives is **not stable**; need stability? sort objects (`Integer[]`) or use a stable algorithm.
  - Comparator `a - b` overflows for `Integer.MIN_VALUE`/`MAX_VALUE` extremes — use `Integer.compare`.
  - Sorting mutates in place; clone if the original order matters.
  - Sorting by the wrong key is the #1 bug in interval problems — be explicit about which field.

## Practice questions
- [[Sort-an-Array]] → implement merge/quick
- [[Merge-Intervals]] → sort by start
- [[Kth-Largest-Element-in-an-Array]] → quickselect / heap
- [[Sort-Colors]] → Dutch national flag (counting/three-way partition)
- [[Top-K-Frequent-Elements]] → freq + bucket/heap

## Related
- [[Arrays]] · [[Divide-and-Conquer]] · [[Heap]] · [[Two-Pointers]] · [[Merge-Intervals]]
