---
type: concept
tags:
  - dsa/algo/greedy
difficulty: medium
pattern: ""
related:
  - "[[Sorting]]"
  - "[[Heap]]"
  - "[[DP]]"
  - "[[Merge-Intervals]]"
aliases:
  - Greedy
  - Greedy algorithm
---

# Greedy

> [!note] Definition
> A greedy algorithm builds a solution by always taking the **locally optimal** choice and never reconsidering. It's fast (often O(n log n) after a sort) but only correct when local optimality provably leads to a global optimum. Otherwise the greedy answer is wrong and you need [[DP]] or [[Backtracking]].

## When greedy works (the hard part)
Greedy is justified by one of:
- **Greedy-choice property** — a locally optimal choice is part of some global optimum.
- **Matroid / exchange argument** — swapping any optimal solution toward the greedy one doesn't worsen it.
- **Optimal substructure** — the remainder after the greedy choice is an optimal subproblem.

If you can't prove one of these, **suspect the greedy** — try a counterexample, then fall back to [[DP]].

## Classic greedy problems
- **Activity selection** — sort by finish time; pick the next compatible activity. (Max count of non-overlapping intervals.)
- **Fractional knapsack** — sort by value/weight; take as much as fits. (Contrast with 0/1 knapsack, which needs [[DP]].)
- **Huffman codes** — repeatedly merge two least-frequent trees ([[Heap]]).
- **Dijkstra** — greedily settle the closest unsettled node ([[Heap]]).
- **Prim/Kruskal MST** — greedily add the cheapest safe edge.
- **Jump game** — greedily extend the farthest reachable index.
- **Gas station / task scheduler** — sort/priority-queue based.
- **Interval scheduling / merging** — sort by start/end → [[Merge-Intervals]].

## Worked: activity selection (max non-overlapping intervals)
```java
int maxIntervals(int[][] iv){
    Arrays.sort(iv, (a,b) -> a[1] - b[1]);   // by END time
    int count = 0, lastEnd = Integer.MIN_VALUE;
    for (int[] x : iv){
        if (x[0] >= lastEnd){ count++; lastEnd = x[1]; }   // take compatible
    }
    return count;
}
```
> [!tip] Why sort by **end**? Taking the interval that finishes earliest leaves maximum room for the rest — the greedy-choice property. Sorting by start or length fails.

## Worked: jump game (greedy reach)
```java
boolean canJump(int[] a){
    int reach = 0;
    for (int i = 0; i < a.length; i++){
        if (i > reach) return false;          // can't get here
        reach = Math.max(reach, i + a[i]);    // greedily extend
    }
    return true;
}
```

## Greedy vs DP decision guide
| Signal | Approach |
|--------|----------|
| Choice with long-range trade-offs, subproblems overlap | [[DP]] |
| Independent local choices, provably monotone order | Greedy |
| "Maximum number of non-overlapping" intervals | Greedy (sort by end) |
| "Maximum value with weight limit, items indivisible" | [[DP]] (0/1 knapsack) |
| "...items divisible" | Greedy (fractional) |
| Coin change with canonical denominations | Greedy; arbitrary denominations → [[DP]] |

> [!tip] Pattern recognition cues
> - "Schedule / select max non-overlapping" → sort + greedy.
> - "Minimum platforms / meeting rooms" → sort starts and ends, sweep.
> - "Reachability in one pass (jump game)" → greedy max-reach.
> - "Cheapest edge / closest node / merge least frequent" → greedy + [[Heap]].
> - "Assign tasks to workers optimally" → sort both, pair extremes.

> [!warning] Pitfalls
  - The biggest trap: **applying greedy without a proof**. Always try to find a counterexample first (e.g. coin change with `{1,3,4}` for 6 → greedy gives 4+1+1=3 coins, optimal is 3+3=2).
  - Wrong sort key (start vs end vs length) flips correctness for interval problems.
  - "Looks greedy" problems that actually need DP: 0/1 knapsack, LIS, edit distance.
  - Greedy may need a [[Heap]] for tie-breaking (task scheduler, reorganize string).

## Practice questions
- [[Jump-Game]] → greedy reach
- [[Non-overlapping-Intervals]] → sort by end, greedy
- [[Assign-Cookies]] → sort + two pointers
- [[Task-Scheduler]] → greedy + math/heap
- [[Gas-Station]] → greedy one pass
- [[Huffman]] / [[Reorganize-String]] → greedy + heap

## Related
- [[Sorting]] · [[Heap]] · [[DP]] · [[Merge-Intervals]]
