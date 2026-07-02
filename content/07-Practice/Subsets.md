---
type: question
tags:
  - dsa/algo/backtracking
  - dsa/pattern/subsets
  - practice
  - practice/medium
  - subsets
difficulty: medium
pattern: "[[Subsets]]"
status: evergreen
source: "LeetCode 78"
related:
  - "[[Subsets]]"
  - "[[Backtracking]]"
  - "[[Subsets-II]]"
aliases:
  - Subsets
  - Power Set
---


# Subsets

> [!note] Problem
> Given an integer array `nums` with **distinct** elements, return all possible subsets (the power set). No duplicate subsets.

## Examples
```
Input:  nums = [1,2,3]
Output: [[],[1],[2],[3],[1,2],[1,3],[2,3],[1,2,3]]
```

## Brute force
- N/A — enumeration is the goal.

## Optimal approach
- Pattern: [[Subsets]] (backtracking, choose / un-choose)
- Idea: at each index decide include/exclude; record at the leaves (or at every node).
- Steps:
  1. `backtrack(start, cur)`: record `cur` (copy); for `i` in `start..n-1`: add `nums[i]`, recurse from `i+1`, remove it.

## Complexity
- Time: O(2ⁿ · n) (2ⁿ subsets, up to n to copy each).
- Space: O(2ⁿ · n) output; O(n) recursion depth.

## Java solution (for-loop form, records every prefix)
```java
public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> out = new ArrayList<>();
    backtrack(nums, 0, new ArrayList<>(), out);
    return out;
}
private void backtrack(int[] nums, int start, List<Integer> cur, List<List<Integer>> out) {
    out.add(new ArrayList<>(cur));                      // record current subset
    for (int i = start; i < nums.length; i++) {
        cur.add(nums[i]);
        backtrack(nums, i + 1, cur, out);
        cur.remove(cur.size() - 1);                     // un-choose
    }
}
```

## Iterative alternative (doubling)
```java
List<List<Integer>> out = new ArrayList<>(); out.add(new ArrayList<>());
for (int x : nums) {
    int size = out.size();
    for (int i = 0; i < size; i++) {
        List<Integer> copy = new ArrayList<>(out.get(i));
        copy.add(x); out.add(copy);
    }
}
```

> [!tip] Always copy when recording (`new ArrayList<>(cur)`); never store the live list — it keeps mutating through the backtrack.

## Related
- [[Subsets]] · [[Backtracking]] · [[Subsets-II]] (with duplicates) · [[Combinations]]
