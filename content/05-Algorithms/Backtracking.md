---
type: concept
tags:
  - dsa/algo/backtracking
difficulty: hard
pattern: ""
related:
  - "[[Recursion]]"
  - "[[Subsets]]"
  - "[[Tree-DFS]]"
  - "[[Graph]]"
  - "[[Trie]]"
aliases:
  - Backtracking
  - DFS with undo
---

# Backtracking

> [!note] Definition
> Backtracking is a depth-first exploration of a **decision space**: at each step make a choice, recurse, then **undo** the choice to try alternatives. It systematically searches all candidate solutions, pruning infeasible branches early. Built on [[Recursion]].

## The template
```java
void backtrack(state){
    if (isSolution(state)){ record(state); return; }
    for (choice : choices(state)){
        if (!valid(choice)) continue;        // prune
        make(choice);                        // choose
        backtrack(newState);                 // explore
        undo(choice);                        // un-choose
    }
}
```
Four ingredients: **state**, **choices**, **validity/pruning**, **make/undo**.

## Worked: permutations
```java
void perm(int[] a, int idx, List<List<Integer>> out){
    if (idx == a.length){ List<Integer> c=new ArrayList<>(); for(int x:a)c.add(x); out.add(c); return; }
    for (int i = idx; i < a.length; i++){
        swap(a, idx, i);                 // make
        perm(a, idx+1, out);
        swap(a, idx, i);                 // undo
    }
}
```

## Worked: N-Queens (classic pruning)
```java
void solve(int row, int n, boolean[] col, boolean[] d1, boolean[] d2, int[] count){
    if (row == n){ count[0]++; return; }
    for (int c = 0; c < n; c++){
        if (col[c] || d1[row+c] || d2[row-c+n]) continue;   // prune attacked squares
        col[c]=d1[row+c]=d2[row-c+n]=true;                  // make
        solve(row+1, n, col, d1, d2, count);
        col[c]=d1[row+c]=d2[row-c+n]=false;                 // undo
    }
}
```
The diagonal encoding `row+c` and `row-c+n` maps each diagonal to a unique index — the key pruning trick.

## Complexity
- Usually exponential (you're enumerating a combinatorial space): permutations O(n!), subsets O(2ⁿ).
- **Pruning** is what makes backtracking practical (cut branches that can't lead to a solution).
- Order choices to fail fast (most-constrained-first) → bigger pruning → orders-of-magnitude speedups.

## When to use
- Enumerate all permutations / combinations / subsets.
- Constraint satisfaction: N-Queens, Sudoku, crossword.
- Path/maze exploration with obstacles.
- Word search on a grid → DFS + trie ([[Trie]] + [[Backtracking]]).
- Partition problems (palindrome partitioning, partition-into-k-equal-sums).

## Pruning patterns
- **Validity check** before recursing (queens not attacked, sudoku cell legal).
- **Bound/estimate** (branch-and-bound): if the best possible completion can't beat the current best, cut.
- **Symmetry breaking**: skip choices equivalent to ones already tried (avoids duplicate permutations when input has duplicates — `if (i>idx && a[i]==a[i-1]) continue;` after sorting).

> [!tip] Pattern recognition cues
> - "Generate all / enumerate / list every ..." → backtracking.
> - "Place items under constraints (queens, sudoku)" → backtracking with pruning.
> - "Path in a maze / word on a grid" → DFS + visited + undo.
> - "Combinations/subsets/permutations" → backtracking → [[Subsets]] pattern.

> [!warning] Pitfalls
  - Forgetting to **undo** (or undoing the wrong thing) → wrong/corrupted state across branches.
  - Mutating shared output instead of copying (`out.add(new ArrayList<>(cur))`, not the live list).
  - No pruning on large spaces → TLE; always prune and pick a good branching order.
  - Visited marking on grids: mark on enter, clear on exit, to allow other paths.
  - Duplicates in input → duplicate outputs; sort and skip equal successors.

## Practice questions
- [[Permutations]] → backtracking
- [[Subsets]] → backtracking / iterative
- [[Combination-Sum]] → backtracking with reuse
- [[N-Queens]] → constraint pruning
- [[Word-Search]] → grid DFS + undo
- [[Sudoku-Solver]] → constraint satisfaction

## Related
- [[Recursion]] · [[Subsets]] · [[Tree-DFS]] · [[Graph]] · [[Trie]]
