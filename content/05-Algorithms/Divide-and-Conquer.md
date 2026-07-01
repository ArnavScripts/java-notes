---
type: concept
tags:
  - dsa/algo/dc
difficulty: medium
pattern: ""
related:
  - "[[Recursion]]"
  - "[[Sorting]]"
  - "[[Searching]]"
  - "[[BST]]"
  - "[[Complexity-Analysis]]"
aliases:
  - Divide and Conquer
  - D&C
---

# Divide and Conquer

> [!note] Definition
> Divide and conquer solves a problem by **splitting** it into independent subproblems, **solving** each recursively, and **combining** the results. Recurrences of the form `T(n) = a·T(n/b) + O(n^k)` analyze via the Master theorem ([[Complexity-Analysis]]).

## The three phases
1. **Divide** — split input into smaller parts (often halves).
2. **Conquer** — recursively solve each part (base case for tiny inputs).
3. **Combine** — merge the partial answers.

## Canonical examples
| Problem | Divide | Combine | Complexity |
|---------|--------|---------|-----------|
| Merge sort | halve array | merge sorted halves | O(n log n) |
| Quicksort | partition by pivot | concatenate | O(n log n) avg |
| Binary search | pick half containing target | — | O(log n) |
| Strassen matrix mult | 4 quadrant mults | combine | O(n^2.81) |
| Closest pair of points | split by x | merge strip | O(n log n) |
| Karatsuba mult | 3 sub-mults | combine | O(n^1.585) |

## Worked: merge sort (see [[Sorting]])
```java
void ms(int[] a, int lo, int hi){
    if (lo >= hi) return;                 // base
    int mid = (lo+hi)/2;
    ms(a, lo, mid); ms(a, mid+1, hi);     // divide + conquer
    merge(a, lo, mid, hi);                // combine
}
```

## Worked: count inversions
An inversion is a pair (i, j) with i < j and a[i] > a[j]. Count during merge:
```java
long mergeCount(int[] a, int lo, int mid, int hi){
    int[] tmp = Arrays.copyOfRange(a, lo, hi+1);
    long inv = 0; int i = 0, j = mid-lo+1, k = lo, size = hi-lo+1;
    int leftLen = mid-lo+1;
    // standard merge, but when taking from right half first, add leftLen-i inversions
    ...
    return inv;
}
```
> [!tip] Counting during the merge step is a classic D&C trick — the combine phase often carries extra "cross" information (inversions, cross pairs).

## Divide and conquer on trees
Tree problems are naturally D&C: answer = combine(left subtree's answer, right subtree's answer). Examples: max depth, diameter, is-balanced, path-sum. See [[BST]] / [[Tree-DFS]].

## D&C on arrays: quickselect
Find k-th smallest by partitioning and recursing only into the side that contains k → average O(n). See [[Sorting]].

## When D&C applies
- Subproblems are **independent** (no overlap — if they overlap, you want [[DP]]).
- The combine step is cheaper than solving the whole directly.
- Input has a natural split (array halves, tree branches, matrix quadrants).

> [!tip] D&C vs DP — the key distinction
> - D&C: subproblems **don't overlap** → solve once, no memoization (mergesort, binary search).
> - DP: subproblems **overlap** → memoize/tabulate to avoid recomputation ([[DP]]).
> If you find yourself recomputing the same subproblem in a D&C recursion tree, switch to DP.

> [!warning] Pitfalls
  - Combine step cost dominates — analyze it (Master theorem), don't assume "halving = log n".
  - Overlapping subproblems masquerading as D&C → exponential blow-up (naive Fibonacci) → use DP.
  - Off-by-one in partitioning (quicksort/quickselect) → infinite loops or wrong answers.
  - Recursion depth = log n for balanced splits (safe) but O(n) for skewed (stack risk).

## Practice questions
- [[Sort-an-Array]] → merge/quick
- [[Majority-Element]] → D&C (or Boyer-Moore)
- [[Merge-K-Sorted-Lists]] → D&C merge (or [[K-Way-Merge]])
- [[Count-of-Smaller-Numbers-After-Self]] → merge sort inversion variant
- [[Maximum-Depth-of-Binary-Tree]] → tree D&C

## Related
- [[Recursion]] · [[Sorting]] · [[Searching]] · [[BST]] · [[Complexity-Analysis]] · [[DP]]
