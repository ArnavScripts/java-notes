---
type: concept
tags:
  - dsa/algo/recursion
difficulty: medium
pattern: ""
related:
  - "[[Backtracking]]"
  - "[[Divide-and-Conquer]]"
  - "[[DP]]"
  - "[[Tree-DFS]]"
  - "[[JVM-and-Memory]]"
aliases:
  - Recursion
  - Call stack
  - Recursion tree
---

# Recursion

> [!note] Definition
> A function that calls itself to solve a smaller instance of the same problem. Every recursion needs: a **base case** (stop) and a **recursive case** (progress toward the base). Each call creates a **stack frame**; deep recursion can overflow the stack.

## The three parts
1. **Base case(s)** — the trivial case(s) that return directly.
2. **Recursive case** — reduce the input and recurse.
3. **Combine** — assemble the answer from the recursive result(s).

```java
int fact(int n){
    if (n <= 1) return 1;          // base case
    return n * fact(n - 1);        // recursive case + combine
}
```

## The call stack
`fact(4)` → `fact(3)` → `fact(2)` → `fact(1)`. Frames pile up; each returns and unwinds. Space = O(depth); time = sum of work across all frames.

## Recursion tree & complexity
For branching recursion (e.g. naive Fibonacci `f(n)=f(n-1)+f(n-2)`):
- Tree has up to 2^depth nodes → O(2ⁿ) time, O(n) stack space.
- Overlapping subproblems → memoize → [[DP]].
- Balanced divide-and-conquer (`f(n)=2f(n/2)+O(n)`) → O(n log n) → [[Divide-and-Conquer]].

## Tail recursion
A call whose recursive step is the **last** operation (no combine). Java does **not** guarantee tail-call optimization → still consumes stack. Convert to iteration for safety on large inputs.

## Converting recursion → iteration
- Use an explicit `ArrayDeque` stack to mimic the call stack ([[Tree-DFS]]).
- Tail-recursive logic → a `while` loop with updated parameters.

## When recursion shines
- Tree/graph traversal (natural recursive structure) → [[Tree-DFS]].
- **Divide and conquer** (merge sort, quickselect) → [[Divide-and-Conquer]].
- **Backtracking** (permutations, subsets, sudoku) → [[Backtracking]].
- Problems defined recursively (Fib, power set, BST ops).
- Where memoization converts it to [[DP]].

## Worked: generate all subsets
```java
void subsets(int[] a, int i, List<Integer> cur, List<List<Integer>> out){
    if (i == a.length){ out.add(new ArrayList<>(cur)); return; }   // base
    cur.add(a[i]);           // choose
    subsets(a, i+1, cur, out);
    cur.remove(cur.size()-1);// un-choose
    subsets(a, i+1, cur, out);
}
```
> [!tip] This is the seed of [[Backtracking]] and [[Subsets]] — choose / recurse / un-choose.

> [!warning] Pitfalls
  - Missing / unreachable base case → infinite recursion → `StackOverflowError` ([[JVM-and-Memory]]).
  - Not progressing toward the base case (e.g. `f(n)=f(n)`).
  - Java stack default ~512KB → ~10⁴–10⁵ frames; deep recursion on n=10⁶ overflows → iterate or raise `-Xss`.
  - Repeating identical subproblems → exponential blow-up → add memoization ([[DP]]).

## Practice questions
- [[Climbing-Stairs]] → recursion → memoize (DP)
- [[Subsets]] → recursion / backtracking
- [[Generate-Parentheses]] → backtracking
- [[Permutations]] → backtracking
- [[Maximum-Depth-of-Binary-Tree]] → tree recursion

## Related
- [[Backtracking]] · [[Divide-and-Conquer]] · [[DP]] · [[Tree-DFS]] · [[JVM-and-Memory]]
