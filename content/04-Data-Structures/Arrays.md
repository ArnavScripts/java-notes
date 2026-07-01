---
type: concept
tags:
  - java/ds/array
difficulty: easy
pattern: ""
related:
  - "[[Arrays-Basics]]"
  - "[[Strings-DS]]"
  - "[[Two-Pointers]]"
  - "[[Sliding-Window]]"
  - "[[Prefix-Sum]]"
  - "[[Cyclic-Sort]]"
  - "[[In-Place-Reversal]]"
  - "[[Sorting]]"
  - "[[Searching]]"
aliases:
  - Array DS
---

# Arrays

> [!note] Definition
> An array is a contiguous, fixed-length block storing same-type elements in O(1) indexed access. It is the **foundation** of most DSA patterns: nearly every "two-pointer", "sliding window", "prefix sum", or "in-place" problem lives on an array.

## Core operations & complexity
| Op | Time |
|----|------|
| `a[i]` get/set | O(1) |
| `length` | O(1) |
| Insert/delete at end (if room) | O(1) |
| Insert/delete middle | O(n) (shift) |
| Search unsorted | O(n) |
| Search sorted | O(log n) — binary search |

## Worked pattern: in-place reverse
```java
void reverse(int[] a) {
    int l = 0, r = a.length - 1;
    while (l < r) { int t = a[l]; a[l++] = a[r]; a[r--] = t; }
}
```
> [!tip] This is the seed of [[Two-Pointers]] and [[In-Place-Reversal]].

## Worked pattern: prefix sums
```java
int[] p = new int[n+1];
for (int i = 0; i < n; i++) p[i+1] = p[i] + a[i];
int rangeSum(int l, int r) { return p[r+1] - p[l]; }   // O(1)
```
> [!tip] Foundation of [[Prefix-Sum]] for subarray sums / count queries.

## Sorting unlocks
Once sorted, an array enables [[Two-Pointers]] (two-sum, pair-with-difference), binary search ([[Searching]]), [[Merge-Intervals]], and `Arrays.binarySearch`.

## 2D / matrix
A 2D array is an array of row references → `grid[i][j]` is O(1). Traversal patterns: row-major, spiral, diagonals, BFS over cells ([[Graph-BFS]] treats the grid as a graph). See [[Arrays-Basics]].

## Common Java helpers
```java
Arrays.sort(a);                        // dual-pivot quicksort, O(n log n)
Arrays.sort(arr, (x,y)->x[0]-y[0]);    // sort int[][] by first col
Arrays.binarySearch(a, key);           // requires sorted; returns index or -(ins+1)
Arrays.copyOfRange(a, from, to);       // [from, to)
```

> [!tip] Pattern recognition cues
> - "Sorted array" + "find pair/triplet" → [[Two-Pointers]].
> - "Contiguous subarray" + "max/min/sum/k distinct" → [[Sliding-Window]].
> - "Sum of subarray [l,r]" repeatedly → [[Prefix-Sum]].
> - "Numbers 1..n in array of size n, one missing/duplicate" → [[Cyclic-Sort]].
> - "Rotate / reverse part" → [[In-Place-Reversal]].
> - "Find k-th / median / top-k" → [[Heap]] or quickselect.
> - "Matrix flood fill / shortest path" → [[Graph-BFS]].

> [!warning] Pitfalls
> - Off-by-one on bounds; pick inclusive vs exclusive consistently.
> - Overflow in sums: `a[i] + a[j]` may exceed `int`; use `long`. Same for prefix sums.
> - Sorting mutates in place — clone if you need the original order.

## Practice questions
- [[Two-Sum]] (sorted) → [[Two-Pointers]]
- [[Maximum-Subarray]] → [[Prefix-Sum]] / Kadane
- [[Best-Time-to-Buy-and-Sell-Stock]] → [[Sliding-Window]]-ish (one pass min)
- [[Contains-Duplicate]] → [[HashSet]] / sorting
- [[Product-of-Array-Except-Self]] → prefix/suffix products

## Related
- [[Arrays-Basics]] (syntax) · [[Strings-DS]] · [[Two-Pointers]] · [[Sliding-Window]] · [[Prefix-Sum]] · [[Cyclic-Sort]] · [[In-Place-Reversal]] · [[Sorting]] · [[Searching]]
