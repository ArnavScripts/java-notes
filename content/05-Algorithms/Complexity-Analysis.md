---
type: concept
tags:
  - dsa/algo
  - dsa/algo/complexity
difficulty: easy
status: evergreen
related:
  - "[[Searching]]"
  - "[[Sorting]]"
  - "[[Recursion]]"
  - "[[DP]]"
aliases:
  - Big O
  - Time complexity
  - Space complexity
---


# Complexity Analysis

> [!note] Definition
> Complexity describes how an algorithm's resource use (time, space) scales with input size n, ignoring constants and lower-order terms. **Big-O** = upper bound (worst case we care about), **Ω** = lower bound, **Θ** = tight bound.

## The hierarchy (most useful)
| Complexity | Example |
|-----------|---------|
| O(1) | hash lookup, array index |
| O(log n) | binary search, balanced BST op |
| O(n) | single pass |
| O(n log n) | comparison sort, merge |
| O(n²) | nested loops, simple DP |
| O(n³) | matrix multiply, Floyd |
| O(2ⁿ) | naive subsets/permutations |
| O(n!) | naive permutations |

> [!tip] Contest budget rule of thumb
> ~10⁸ simple ops/sec → for n=10⁵ you want **O(n log n)** or better; n=1000 allows O(n²); n≤20 allows O(2ⁿ). Always estimate before coding.

## Counting from code
- One loop over n → O(n).
- Nested independent loops → multiply: O(n·m).
- Sequential blocks → take the max.
- Logarithmic: each step halves the input (`while (n>1) n/=2`) → O(log n).
- Recursion with two calls each halving → recurrence `T(n)=2T(n/2)+O(n)` = O(n log n) (merge sort).

## Amortized analysis
Some ops are occasionally expensive but cheap on average:
- `ArrayList.add` → O(1) amortized (doubling resize every 2ᵏ inserts averages out).
- `StringBuilder.append` → O(1) amortized.
- DSU with path compression + union by rank → O(α(n)) amortized ([[Disjoint-Set]]).

## Space complexity
- Auxiliary space = extra memory beyond the input.
- Recursive call stack counts: depth-d recursion = O(d) space.
- In-place vs. copy: [[Two-Pointers]], [[In-Place-Reversal]] use O(1) extra; mergesort needs O(n).

## Solving recurrences
- **Master theorem**: `T(n) = a·T(n/b) + f(n)`.
  - If `f(n) = O(n^c)` with c = log_b a → Θ(n^c log n).
  - c < log_b a → Θ(n^{log_b a}); c > log_b a → Θ(f(n)).
- Common: merge sort `2T(n/2)+O(n)` → O(n log n); binary search `T(n/2)+O(1)` → O(log n); naive fib `2T(n-1)` → O(2ⁿ).

> [!warning] Pitfalls
  - Hidden factors: a "linear" scan with a nested `String.contains` is O(n·m), not O(n).
  - `O(n log n)` for sorting is the **comparison** lower bound; counting/radix beat it with assumptions ([[Sorting]]).
  - Forgetting the recursion stack in space analysis (DFS = O(h) stack).
  - Constants still matter for borderline time limits; O(n) with a huge constant can TLE where O(n log n) with a small one passes.

## Related
- [[Searching]] · [[Sorting]] · [[Recursion]] · [[DP]]
