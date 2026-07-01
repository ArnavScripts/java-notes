---
type: concept
tags:
  - dsa/pattern/two-pointers
difficulty: easy
pattern: "[[Two-Pointers]]"
related:
  - "[[Sliding-Window]]"
  - "[[Fast-Slow-Pointers]]"
  - "[[Arrays]]"
  - "[[Strings-DS]]"
  - "[[Sorting]]"
  - "[[HashMap]]"
aliases:
  - Two Pointers
  - Two pointers
---

# Two Pointers

> [!note] Definition
> Use two indices moving through the data (usually an array/string/list) to find a pair or triple satisfying a condition — typically in **O(n)** instead of O(n²). Three flavors: **convergent** (left/right toward center), **parallel** (both forward), and **fast/slow** (one ahead — see [[Fast-Slow-Pointers]]).

## When it applies (cues)
- Sorted array + "find pair/triplet with sum/target."
- "Find a pair in an array with a property" (often after sorting).
- In-place array modification (remove duplicates, move zeros, dedupe sorted).
- Palindrome check (pointers from both ends inward).
- Linked-list offset tricks (k-th from end).

## Template A — convergent (sorted two-sum)
```java
int[] twoSumSorted(int[] a, int target){
    int l = 0, r = a.length - 1;
    while (l < r){
        int sum = a[l] + a[r];
        if (sum == target) return new int[]{l, r};
        if (sum < target) l++; else r--;
    }
    return new int[]{-1,-1};
}
```
> [!tip] Why sorted? The monotonicity lets you decide which pointer to move: sum too small → need a bigger left; too big → need a smaller right. Unsorted → sort first (O(n log n)) or use a [[HashMap]].

## Template B — parallel (remove duplicates / move zeros)
```java
// move zeros to the end, keep order, in place
void moveZeroes(int[] a){
    int w = 0;                                  // write pointer
    for (int r = 0; r < a.length; r++)
        if (a[r] != 0) a[w++] = a[r];
    while (w < a.length) a[w++] = 0;
}
```
Slow writer + fast reader: the slow pointer marks the next "good" slot.

## Template C — 3-sum (sort + two pointers)
```java
List<List<Integer>> threeSum(int[] a){
    Arrays.sort(a);
    List<List<Integer>> res = new ArrayList<>();
    for (int i = 0; i < a.length - 2; i++){
        if (i > 0 && a[i] == a[i-1]) continue;            // skip duplicate i
        int l = i+1, r = a.length-1;
        while (l < r){
            int s = a[i]+a[l]+a[r];
            if (s == 0){ res.add(List.of(a[i],a[l],a[r])); 
                int lv=a[l], rv=a[r]; while (l<r&&a[l]==lv)l++; while (l<r&&a[r]==rv)r--; }
            else if (s < 0) l++; else r--;
        }
    }
    return res;
}
```
> [!tip] The duplicate-skip (`if (i>0 && a[i]==a[i-1]) continue;` and skipping equal values after a match) is the key to avoiding duplicate triplets.

## Template D — palindrome
```java
boolean isPal(String s, int l, int r){
    while (l < r) if (s.charAt(l++) != s.charAt(r--)) return false;
    return true;
}
```

## Complexity
- Time: O(n) (single pass) or O(n log n) if a sort is needed first.
- Space: O(1) — the hallmark advantage.

## Variants & neighbors
- **Two pointers on two arrays** (intersection/merge): advance the pointer pointing at the smaller value.
- **k-th from end of a list**: leader k ahead, then walk both → [[Fast-Slow-Pointers]]-adjacent.
- **Container with most water**: convergent pointers; move the shorter line.
- **Trapping rain water**: convergent with running max from each side.
- **Sorted squares / Dutch flag**: parallel or three-way partition.

> [!warning] Pitfalls
  - Applying to **unsorted** data without sorting → wrong logic (the "move which pointer?" decision breaks).
  - Off-by-one on `l < r` vs `l <= r` and which pointer to move on equality (depends on whether you need all pairs or one).
  - Duplicate outputs in 3-sum → skip equal neighbors after recording a match.
  - Overflow in `a[l]+a[r]` for large ints → use `long`.

## Practice questions
- [[Two-Sum]] (sorted variant) / [[Two-Sum-II-Input-Array-Is-Sorted]]
- [[3Sum]] → sort + two pointers
- [[Container-With-Most-Water]] → convergent
- [[Trapping-Rain-Water]] → convergent / prefix max
- [[Move-Zeroes]] → parallel
- [[Valid-Palindrome]] → convergent
- [[Remove-Duplicates-from-Sorted-Array]] → parallel

## Related
- [[Sliding-Window]] (a "flexible window" is two pointers) · [[Fast-Slow-Pointers]] · [[Arrays]] · [[Strings-DS]] · [[Sorting]] · [[Prefix-Sum]]
