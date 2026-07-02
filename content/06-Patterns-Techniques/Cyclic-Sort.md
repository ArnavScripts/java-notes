---
type: concept
tags:
  - dsa/pattern
  - dsa/pattern/cyclic-sort
difficulty: medium
pattern: "[[Cyclic-Sort]]"
status: evergreen
related:
  - "[[Arrays]]"
  - "[[HashSet]]"
  - "[[Bit-Manipulation]]"
aliases:
  - Cyclic Sort
  - Cycle sort
  - Index marking
---


# Cyclic Sort

> [!note] Definition
> When an array of length n contains values in a known range (typically `1..n` or `0..n-1`), each value "wants" to sit at index `value - 1`. Swap elements to their correct positions in O(n), then a second pass reveals missing/duplicate/all-missing numbers — all in **O(n) time, O(1) space**.

## When it applies (cues)
- "Array of n integers in range 1..n, find the missing one(s) / duplicate(s)."
- "Find all numbers disappeared in an array."
- "Find the first missing positive" (after normalizing negatives/zeros).
- "Find the corrupt pair (one missing, one duplicate)."
- Any problem where **value range == index range** and O(1) extra space is required.

## Template — cyclic sort
```java
void cyclicSort(int[] a){
    int i = 0, n = a.length;
    while (i < n){
        int correct = a[i] - 1;                 // where a[i] belongs
        if (a[i] > 0 && a[i] <= n && a[i] != a[correct]) {
            int t = a[i]; a[i] = a[correct]; a[correct] = t;   // swap to its place
        } else i++;
    }
}
```
After this, `a[i]` should equal `i+1`; any index where it doesn't flags a missing number (and the value there flags a duplicate).

## Worked — find the one missing (1..n)
```java
int missing(int[] a){
    for (int i = 0; i < a.length; i++)
        while (a[i] != i+1 && a[i] != a[a[i]-1]) swap(a, i, a[i]-1);
    for (int i = 0; i < a.length; i++) if (a[i] != i+1) return i+1;
    return a.length + 1;
}
```

## Worked — find all disappeared
```java
List<Integer> disappeared(int[] a){
    for (int i = 0; i < a.length; i++)
        while (a[i] != a[a[i]-1]) swap(a, i, a[i]-1);
    List<Integer> res = new ArrayList<>();
    for (int i = 0; i < a.length; i++) if (a[i] != i+1) res.add(i+1);
    return res;
}
```

## Alternative — index marking (no swaps, sign flip)
When you **must not** move elements (or values may be out of range), mark visited indices by negating the value at `|a[i]| - 1`. A positive entry after the pass → its index+1 is missing.
```java
int firstMissingPositive(int[] a){
    int n = a.length;
    for (int i = 0; i < n; i++) if (a[i] <= 0 || a[i] > n) a[i] = n + 1;  // sanitize
    for (int i = 0; i < n; i++){
        int idx = Math.abs(a[i]) - 1;
        if (idx < n && a[idx] > 0) a[idx] = -a[idx];                     // mark seen
    }
    for (int i = 0; i < n; i++) if (a[i] > 0) return i + 1;
    return n + 1;
}
```
> [!tip] Index-marking is the cousin technique: instead of moving a value to its index, **flip the sign at its index** to record presence. Both exploit "value → index" mapping for O(1) space.

## Complexity
- Time: O(n) — each swap places one element permanently; at most n swaps total.
- Space: O(1) — in place (ignoring the output list).

> [!warning] Pitfalls
  - Out-of-range values break `a[a[i]-1]` → always guard `a[i] > 0 && a[i] <= n` (and skip via `i++` when not).
  - Infinite loop when a duplicate exists at the target slot → the `a[i] != a[correct]` guard handles it (don't swap equal values; advance `i` instead).
  - Mutating the input — some problems forbid it; then use index-marking with sign flips and restore signs, or accept mutation.
  - Off-by-one between 0-based index and 1-based value: `correct = a[i] - 1`.

## Practice questions
- [[Find-the-Duplicate-Number]] (O(1) space variant; also Floyd's)
- [[First-Missing-Positive]] → index marking
- [[Find-All-Numbers-Disappeared-in-an-Array]] → cyclic sort
- [[Find-the-Corrupt-Pair]] → one missing + one duplicate
- [[Cyclic-Sort]] / [[Missing-Number]] (range 0..n)

## Related
- [[Arrays]] · [[HashSet]] · [[Bit-Manipulation]] (XOR for single missing)
