---
type: concept
tags:
  - dsa/algo/searching
difficulty: easy
pattern: ""
related:
  - "[[Arrays]]"
  - "[[BST]]"
  - "[[Two-Pointers]]"
  - "[[Complexity-Analysis]]"
aliases:
  - Searching
  - Binary Search
  - Linear Search
---

# Searching

> [!note] Definition
> Find an element (or the boundary where a predicate flips) in a collection. Linear search is O(n); **binary search** is O(log n) but requires a **sorted / monotonic** search space.

## Linear search
```java
int indexOf(int[] a, int key){
    for (int i = 0; i < a.length; i++) if (a[i] == key) return i;
    return -1;
}
```
O(n); the only option on unsorted data without preprocessing.

## Binary search — the classic
```java
int bs(int[] a, int key){              // a sorted ascending
    int lo = 0, hi = a.length - 1;
    while (lo <= hi){
        int mid = lo + (hi - lo) / 2;  // avoid (lo+hi) overflow
        if (a[mid] == key) return mid;
        if (a[mid] < key) lo = mid + 1; else hi = mid - 1;
    }
    return -1;
}
```

## Binary search on the answer (key idea!)
> [!tip] The most powerful use: when a predicate `check(x)` is **monotonic** (false…false true…true), binary search the smallest x with `check(x) == true`. This turns "find the optimum" into O(log range · cost of check).

```java
int lo = minPossible, hi = maxPossible, ans = -1;
while (lo <= hi){
    int mid = lo + (hi - lo) / 2;
    if (feasible(mid)){ ans = mid; hi = mid - 1; }   // looking for min feasible
    else lo = mid + 1;
}
```
Used for: split-array-largest-sum, capacity-to-ship-packages, min-time-to-make-m bouquets, koko-eating-bananas, aggressive-cows.

## Lower / upper bound
```java
// first index where a[i] >= key  (lower bound)
int lb(int[] a, int key){
    int lo = 0, hi = a.length;
    while (lo < hi){ int mid = (lo+hi)/2; if (a[mid] < key) lo = mid+1; else hi = mid; }
    return lo;
}
```
`Arrays.binarySearch(a, key)` returns `-(insertion point) - 1` if absent — convert: `idx < 0 ? -(idx+1) : idx`.

## Searching in other structures
- **BST**: search by navigating left/right — O(h) ([[BST]]).
- **Sorted matrix**: staircase search from top-right corner → O(m+n).
- **Rotated sorted array**: modified binary search comparing `a[mid]` with `a[lo]`.
- **Peak finding**: binary search toward the rising neighbor.
- **HashMap lookup**: O(1) "search" by key ([[HashMap]]).

> [!tip] Pattern recognition cues
> - "Sorted array" / "find in log time" / "minimize maximum" / "is x enough?" → binary search.
> - "Capacity / threshold / answer is an integer in a range, feasibility is monotone" → **binary search on answer**.
> - "Rotated/pivoted sorted" → modified binary search.

> [!warning] Pitfalls
  - `(lo + hi)` overflow → use `lo + (hi - lo) / 2` or `(lo + hi) >>> 1`.
  - Off-by-one on inclusive vs exclusive `hi`; pick a style (`lo <= hi` with `mid ± 1`, or `lo < hi` with `hi = mid`) and stick to it.
  - Binary search requires the predicate to be monotonic; verify before applying.
  - Duplicate keys: classic `bs` returns any match; use lower/upper bound for first/last.

## Practice questions
- [[Binary-Search]] → classic
- [[Search-in-Rotated-Sorted-Array]] → modified binary search
- [[Find-First-and-Last-Position]] → lower/upper bound
- [[Koko-Eating-Bananas]] → binary search on answer
- [[Split-Array-Largest-Sum]] → binary search on answer / DP

## Related
- [[Arrays]] · [[BST]] · [[Two-Pointers]] · [[Complexity-Analysis]]
