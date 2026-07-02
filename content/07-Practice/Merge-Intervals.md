---
type: question
tags:
  - dsa/algo/sorting
  - dsa/pattern/merge-intervals
  - intervals
  - practice
  - practice/medium
difficulty: medium
pattern: "[[Merge-Intervals]]"
status: evergreen
source: "LeetCode 56"
related:
  - "[[Merge-Intervals]]"
  - "[[Sorting]]"
  - "[[Insert-Interval]]"
aliases:
  - Merge Intervals
---


# Merge Intervals

> [!note] Problem
> Given an array of intervals `intervals[i] = [start, end]`, merge all overlapping intervals and return the non-overlapping set covering the same spans.

## Examples
```
Input:  [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
```

## Brute force
- Compare every pair, merge on overlap, repeat until stable → O(n²).

## Optimal approach
- Pattern: [[Merge-Intervals]] (sort by start)
- Idea: after sorting by start, a single left-to-right pass merges a new interval into the last output interval iff `last.end >= cur.start`.
- Steps:
  1. Sort by `start`.
  2. For each interval: if no overlap with the last in result → push; else extend `last.end = max(last.end, cur.end)`.

## Complexity
- Time: O(n log n) (sort) · Space: O(n) output

## Java solution
```java
public int[][] merge(int[][] intervals) {
    Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
    List<int[]> res = new ArrayList<>();
    for (int[] cur : intervals) {
        if (res.isEmpty() || res.get(res.size() - 1)[1] < cur[0])
            res.add(cur);                                   // no overlap
        else
            res.get(res.size() - 1)[1] = Math.max(res.get(res.size() - 1)[1], cur[1]);
    }
    return res.toArray(new int[0][]);
}
```

> [!tip] Overlap test (sorted by start): `prev.end < cur.start` ⇒ no overlap. The merged end is `max(prev.end, cur.end)` because a later interval can be **fully inside** the running one (e.g. `[1,5]` then `[2,3]`).

## Related
- [[Merge-Intervals]] · [[Sorting]] · [[Insert-Interval]] · [[Meeting-Rooms-II]]
