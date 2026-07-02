---
type: concept
tags:
  - dsa/pattern
  - dsa/pattern/prefix-sum
difficulty: medium
pattern: "[[Prefix-Sum]]"
status: evergreen
related:
  - "[[Arrays]]"
  - "[[HashMap]]"
  - "[[Sliding-Window]]"
aliases:
  - Prefix Sum
  - Cumulative sum
  - Difference array
---


# Prefix Sum

> [!note] Definition
> Precompute cumulative sums so any **range sum** `[l..r]` is answered in O(1) as `prefix[r+1] - prefix[l]`. Generalizes to: subarray-sum-equals-target via a [[HashMap]] of prefix sums; 2D prefix sums for submatrix sums; difference arrays for range **updates**.

## When it applies (cues)
- "Sum of subarray [l..r]" (many queries).
- "Count subarrays with sum = k / divisible by k / at most k."
- "Maximum subarray sum" (Kadane — prefix-sum-adjacent).
- "Range add value to a subarray many times, then read final array" → difference array.
- "Submatrix sum" (2D prefix sums).
- "Equilibrium index" / "pivot index" (left sum == right sum).

## Template — 1D prefix sums
```java
int n = a.length;
int[] pref = new int[n+1];                          // pref[0] = 0
for (int i = 0; i < n; i++) pref[i+1] = pref[i] + a[i];
int rangeSum(int l, int r){ return pref[r+1] - pref[l]; }   // inclusive [l..r]
```
> [!tip] The leading `pref[0] = 0` makes `[0..i]` sum = `pref[i+1]` and `[l..r]` = `pref[r+1]-pref[l]` uniform — no edge cases for `l == 0`.

## Worked — subarray sum equals k (with negatives)
```java
int subarraySum(int[] a, int k){
    Map<Integer,Integer> seen = new HashMap<>(); seen.put(0, 1);   // empty prefix
    int sum = 0, count = 0;
    for (int x : a){
        sum += x;
        count += seen.getOrDefault(sum - k, 0);   // # earlier prefixes that pair with this one
        seen.merge(sum, 1, Integer::sum);
    }
    return count;
}
```
> [!tip] The transformation: a subarray ending at i has sum k iff `pref[i] - pref[j] == k` for some earlier j. Counting `pref[i] - k` seen so far gives the answer in O(n) — and works with **negative** numbers (where [[Sliding-Window]] fails).

## Worked — continuous subarray sum divisible by k (size ≥ 2)
```java
boolean checkSubarraySum(int[] a, int k){
    Map<Integer,Integer> first = new HashMap<>(); first.put(0, -1);
    int sum = 0;
    for (int i = 0; i < a.length; i++){
        sum = (sum + a[i]) % k; if (sum < 0) sum += k;   // handle negative mod
        if (first.containsKey(sum)){ if (i - first.get(sum) >= 2) return true; }
        else first.put(sum, i);
    }
    return false;
}
```
> [!tip] Same remainder at two indices ⇒ the subarray between them has sum divisible by k. Mod is the "prefix sum" for divisibility problems.

## Template — 2D prefix sums (submatrix sum)
```java
int[][] p = new int[m+1][n+1];
for (int i = 0; i < m; i++) for (int j = 0; j < n; j++)
    p[i+1][j+1] = a[i][j] + p[i][j+1] + p[i+1][j] - p[i][j];   // inclusion-exclusion
int region(int r1,int c1,int r2,int c2){                         // inclusive
    return p[r2+1][c2+1] - p[r1][c2+1] - p[r2+1][c1] + p[r1][c1];
}
```

## Template — difference array (range updates)
```java
int[] diff = new int[n+1];
void add(int l, int r, int v){ diff[l] += v; diff[r+1] -= v; }   // apply range add
int[] materialize(){                                            // O(n) final array
    int[] a = new int[n]; int run = 0;
    for (int i = 0; i < n; i++){ run += diff[i]; a[i] = run; }
    return a;
}
```
> [!tip] Difference array = "prefix sum in reverse": instead of O(n) per range add, O(1) per add and one O(n) pass to materialize. Classic for "corporate flight bookings", "car pooling", "range addition."

## Complexity
- Build: O(n) (or O(mn) for 2D).
- Query: O(1) range sum; O(1) range update with difference array + O(n) materialize.
- Space: O(n) (O(mn) for 2D), often reducible if online.

> [!warning] Pitfalls
  - **Overflow**: prefix sums of large arrays/values → use `long`.
  - Index alignment: `pref[i+1]` vs `pref[i]` is the #1 off-by-one; pick the 1-based-pref convention and stick to it.
  - Forgetting `seen.put(0, 1)` in subarray-sum-equals-k (handles subarrays starting at index 0).
  - Negative modulo: in Java `(-7) % 5 == -2`; normalize with `((x % k) + k) % k`.
  - Difference array bounds: size `n+1` so `diff[r+1]` is valid when `r == n-1`.

## Practice questions
- [[Running-Sum-of-1d-Array]] → build prefix
- [[Range-Sum-Query-Immutable]] → range sum
- [[Subarray-Sum-Equals-K]] → hashmap of prefixes
- [[Continuous-Subarray-Sum]] → mod prefix
- [[Pivot-Index]] / [[Find-Pivot-Index]] → left vs right sum
- [[Maximum-Subarray]] → Kadane (running min-prefix)
- [[Range-Addition-II]] / [[Car-Pooling]] → difference array
- [[Range-Sum-Query-2D-Immutable]] → 2D prefix

## Related
- [[Arrays]] · [[HashMap]] · [[Sliding-Window]]
