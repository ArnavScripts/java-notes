---
type: concept
tags:
  - dsa/algo
  - dsa/algo/dp
difficulty: hard
status: evergreen
related:
  - "[[Recursion]]"
  - "[[Divide-and-Conquer]]"
  - "[[Greedy]]"
  - "[[Backtracking]]"
  - "[[HashMap]]"
aliases:
  - DP
  - Dynamic Programming
  - Memoization
  - Tabulation
---


# Dynamic Programming (DP)

> [!note] Definition
> DP optimizes recursion when subproblems **overlap** and the problem has **optimal substructure**. Two implementations: **memoization** (top-down recursion + cache) and **tabulation** (bottom-up table fill). It trades time for space by remembering answers.

## The two ingredients
1. **Overlapping subproblems** — the same subproblem recurs (naive recursion recomputes it exponentially).
2. **Optimal substructure** — an optimal solution can be built from optimal solutions of subproblems.

## Two styles
**Top-down (memoization)** — recursion + a cache:
```java
int[] memo;
int fib(int n){
    if (n <= 1) return n;
    if (memo[n] != 0) return memo[n];
    return memo[n] = fib(n-1) + fib(n-2);
}
```
**Bottom-up (tabulation)** — fill a table from smallest to largest:
```java
int[] dp = new int[n+1];
dp[0]=0; dp[1]=1;
for (int i = 2; i <= n; i++) dp[i] = dp[i-1] + dp[i-2];
```
> [!tip] Top-down is easier to derive from the recurrence; bottom-up is usually faster (no recursion overhead) and easier to space-optimize.

## The DP workflow
1. **Define the state** — what subproblem does `dp[...]` represent? Be precise about indices (inclusive/exclusive).
2. **Recurrence** — how does `dp[i]` (or `dp[i][j]`) relate to smaller states?
3. **Base cases** — smallest states with known answers.
4. **Order of computation** — ensure dependencies are computed first.
5. **Answer extraction** — where in the table is the final answer?
6. **Space optimization** — keep only the last few rows/variables if the recurrence has small width.

## Worked: 0/1 knapsack (2D → 1D)
State: `dp[i][w]` = max value using first i items with capacity w.
```java
int[] dp = new int[W+1];
for (int i = 0; i < n; i++)
    for (int w = W; w >= weight[i]; w--)        // reverse to avoid reuse (0/1)
        dp[w] = Math.max(dp[w], dp[w-weight[i]] + value[i]);
return dp[W];
```
> [!tip] Iterating `w` descending gives **0/1** (each item once); ascending gives **unbounded** knapsack. One loop direction flips the semantics.

## Worked: longest common subsequence
```java
int[][] dp = new int[m+1][n+1];
for (int i = 1; i <= m; i++)
    for (int j = 1; j <= n; j++)
        dp[i][j] = a[i-1]==b[j-1] ? dp[i-1][j-1]+1 : Math.max(dp[i-1][j], dp[i][j-1]);
return dp[m][n];
```

## DP categories (with examples)
| Category | Examples | State shape |
|----------|----------|-------------|
| 1D linear | Climbing stairs, house robber, LIS | `dp[i]` |
| 2D grid | Unique paths, min path sum, dungeon | `dp[i][j]` |
| Two strings | LCS, edit distance, regex | `dp[i][j]` over both |
| Interval / range | Matrix chain mul, burst balloons, stone game | `dp[l][r]` |
| Knapsack / subset | 0/1 knapsack, subset sum, partition equal sum | `dp[i][w]` → 1D |
| Bitmask DP | TSP, assignment, small-set state | `dp[mask][i]` |
| Tree DP | max path sum, house robber III | post-order returns state |
| DP on DAG | LIS as DAG, word break | topological order |
| Digit DP | count numbers in range with property | `pos, tight, ...` |

## Space optimization
If `dp[i]` depends only on `dp[i-1]` (or a fixed window), drop to a 1D array or a few variables:
- Fibonacci → two variables.
- Knapsack → 1D reverse loop.
- LCS → two rows.

> [!tip] Pattern recognition cues
> - "Count ways / number of distinct ..." → DP counting.
> - "Max/min over choices with overlapping subproblems" → DP optimization.
> - "Is it possible to ... (feasibility)" → boolean DP (subset sum, partition, word break).
> - "Sequence with constraint on previous step(s)" → DP on index + last-choice.
> - "Shortest/longest common/aligned subsequence" → 2-string DP.
> - "Partition into k equal / best split" → interval/knapsack DP.
> - "Small n (≤20) + permutations/assignment" → bitmask DP.
> - Greedy fails on counterexamples → DP.

> [!warning] Pitfalls
  - **State definition ambiguity** — be explicit: is `dp[i]` "best using first i" or "best ending exactly at i"? Different recurrences.
  - Wrong iteration order → using not-yet-computed values. Tabulate in dependency order.
  - Integer overflow in counting DP → use `long`.
  - Base cases off by index alignment (LCS uses 1-based dp with 0-based strings).
  - Recursion + memoization on n=10⁵ → stack overflow; switch to bottom-up.
  - Trying every state when many are unreachable → sparse DP or pruning.

## Practice questions
- [[Climbing-Stairs]] → 1D
- [[House-Robber]] → 1D with choice
- [[Longest-Increasing-Subsequence]] → 1D / patience
- [[Coin-Change]] → unbounded knapsack
- [[Longest-Common-Subsequence]] → 2-string
- [[Edit-Distance]] → 2-string
- [[0-1-Knapsack]] → knapsack
- [[Word-Break]] → feasibility DP
- [[Burst-Balloons]] → interval DP
- [[Partition-Equal-Subset-Sum]] → subset sum

## Related
- [[Recursion]] · [[Divide-and-Conquer]] · [[Greedy]] · [[Backtracking]] · [[HashMap]]
