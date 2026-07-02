---
type: concept
tags:
  - dsa/pattern
  - dsa/pattern/subsets
difficulty: medium
pattern: "[[Subsets]]"
status: evergreen
related:
  - "[[Backtracking]]"
  - "[[Recursion]]"
  - "[[Bit-Manipulation]]"
aliases:
  - Subsets
  - Power set
  - Combinations
  - Permutations pattern
---


# Subsets

> [!note] Definition
> Generate the **power set** (all subsets), combinations, or permutations by systematically including/excluding each element. Two implementations: **recursive backtracking** (choose / un-choose) and **iterative expansion** (start with `[]`, push each element into every existing subset). Both run in O(2ⁿ·n) for subsets.

## When it applies (cues)
- "Generate all subsets / power set."
- "All combinations of size k."
- "All permutations."
- "Letter case permutations / string subsets."
- "Subsets with a constraint" (sum, distinct) → subset-style DFS + prune.
- "Target sum from candidates" ([[Combination-Sum]]) → choose/un-choose with reuse control.
- "Brace expansion" / "generate all strings of a pattern".

## Template A — recursive backtracking (subsets)
```java
List<List<Integer>> subsets(int[] a){
    List<List<Integer>> out = new ArrayList<>();
    backtrack(a, 0, new ArrayList<>(), out);
    return out;
}
void backtrack(int[] a, int i, List<Integer> cur, List<List<Integer>> out){
    if (i == a.length){ out.add(new ArrayList<>(cur)); return; }   // record
    cur.add(a[i]);              // include a[i]
    backtrack(a, i+1, cur, out);
    cur.remove(cur.size()-1);   // exclude a[i]
    backtrack(a, i+1, cur, out);
}
```
Alternative: record at **every** node (not just leaves):
```java
void backtrack(int[] a, int start, List<Integer> cur, List<List<Integer>> out){
    out.add(new ArrayList<>(cur));                       // record every prefix
    for (int i = start; i < a.length; i++){
        cur.add(a[i]); backtrack(a, i+1, cur, out); cur.remove(cur.size()-1);
    }
}
```
> [!tip] The for-loop form records all subsets along the way and is the most reusable: same skeleton handles combinations (loop bound), permutations (swap or used[]), and subset-with-duplicates (skip equal).

## Template B — iterative expansion
```java
List<List<Integer>> subsetsIter(int[] a){
    List<List<Integer>> out = new ArrayList<>(); out.add(new ArrayList<>());
    for (int x : a){
        int size = out.size();
        for (int i = 0; i < size; i++){                  // clone each existing subset + add x
            List<Integer> copy = new ArrayList<>(out.get(i));
            copy.add(x); out.add(copy);
        }
    }
    return out;
}
```
> [!tip] Doubling each step → 2ⁿ subsets. Same idea generates letter-case permutations (branch each letter into lower/upper).

## Template C — permutations (swap)
```java
void permute(int[] a, int idx, List<List<Integer>> out){
    if (idx == a.length){ List<Integer> c=new ArrayList<>(); for(int x:a)c.add(x); out.add(c); return; }
    for (int i = idx; i < a.length; i++){
        swap(a, idx, i); permute(a, idx+1, out); swap(a, idx, i);
    }
}
```
With duplicates → sort then skip equal successors: `if (i>idx && a[i]==a[i-1]) continue;`.

## Template D — combinations of size k
```java
void combine(int n, int k, int start, List<Integer> cur, List<List<Integer>> out){
    if (cur.size() == k){ out.add(new ArrayList<>(cur)); return; }
    for (int i = start; i <= n; i++){
        cur.add(i); combine(n, k, i+1, cur, out); cur.remove(cur.size()-1);
    }
}
```

## Template E — combination sum (with reuse)
```java
void comboSum(int[] c, int target, int start, List<Integer> cur, List<List<Integer>> out){
    if (target == 0){ out.add(new ArrayList<>(cur)); return; }
    if (target < 0) return;
    for (int i = start; i < c.length; i++){               // i (not i+1) allows reuse
        cur.add(c[i]); comboSum(c, target - c[i], i, cur, out); cur.remove(cur.size()-1);
    }
}
```
> [!tip] Reuse vs not: recurse with `i` to allow the same element again; with `i+1` to use each once. Sort first to prune on `target < 0` and to skip duplicates.

## Bitmask approach (subsets only, n ≤ ~20)
```java
for (int mask = 0; mask < (1 << n); mask++){
    List<Integer> sub = new ArrayList<>();
    for (int i = 0; i < n; i++) if ((mask & (1<<i)) != 0) sub.add(a[i]);
    out.add(sub);
}
```
> [!tip] Bitmask enumeration maps cleanly to [[Bit-Manipulation]] and to bitmask DP.

## Complexity
- Subsets: O(2ⁿ·n) time, O(2ⁿ·n) output space.
- Permutations: O(n!·n).
- Combinations C(n,k): O(C(n,k)·k).

> [!warning] Pitfalls
  - Recording the **live** `cur` list instead of a copy → all outputs end up empty/mutated. Always `new ArrayList<>(cur)`.
  - Forgetting to backtrack (remove the last element) → wrong supersets.
  - Duplicate outputs when input has duplicates → sort + skip equal successors (`if (i>start && a[i]==a[i-1]) continue;`).
  - Reuse control: `i` vs `i+1` flips between unbounded and 0/1 use.
  - n too large (n > ~20 for subsets, > ~10 for permutations) → exponential blow-up; re-read the problem for a DP/greedy reformulation.

## Practice questions
- [[Subsets]] → power set
- [[Subsets-II]] → with duplicates
- [[Permutations]] / [[Permutations-II]]
- [[Combinations]] → size-k
- [[Combination-Sum]] / [[Combination-Sum-II]] / [[Combination-Sum-III]]
- [[Letter-Case-Permutation]]
- [[Generate-Parentheses]] → backtracking (Catalan-shaped)

## Related
- [[Backtracking]] · [[Recursion]] · [[Bit-Manipulation]] (bitmask subsets)
