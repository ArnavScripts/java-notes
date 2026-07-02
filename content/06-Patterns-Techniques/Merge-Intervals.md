---
type: concept
tags:
  - dsa/pattern
  - dsa/pattern/intervals
difficulty: medium
pattern: "[[Merge-Intervals]]"
status: evergreen
related:
  - "[[Sorting]]"
  - "[[Greedy]]"
  - "[[Arrays]]"
aliases:
  - Merge Intervals
  - Intervals
---


# Merge Intervals

> [!note] Definition
> A family of problems over ranges `[start, end)`: merge overlapping ones, insert a new interval, check conflicts, count concurrent uses (meeting rooms), find the maximum overlap. The universal first step: **sort by start**.

## When it applies (cues)
- "Given a list of intervals, merge overlapping."
- "Insert a new interval into a sorted set."
- "Minimum number of meeting rooms / platforms."
- "Employee free time" (gaps between merged intervals).
- "Can a person attend all meetings?" (any overlap?).
- "Remove the minimum intervals to make non-overlapping."

## Template — merge overlapping
```java
int[][] merge(int[][] iv){
    Arrays.sort(iv, (a,b) -> a[0] - b[0]);     // sort by start
    List<int[]> res = new ArrayList<>();
    for (int[] cur : iv){
        if (res.isEmpty() || res.get(res.size()-1)[1] < cur[0])
            res.add(cur);                       // no overlap -> new block
        else
            res.get(res.size()-1)[1] = Math.max(res.get(res.size()-1)[1], cur[1]); // extend
    }
    return res.toArray(new int[0][]);
}
```
> [!tip] Overlap test (sorted by start): `prev.end < cur.start` → **no overlap**; else overlap, and the merged end is `max(prev.end, cur.end)`.

## Template — insert interval (sorted input)
```java
int[][] insert(int[][] iv, int[] ni){
    List<int[]> res = new ArrayList<>();
    int i = 0, n = iv.length;
    while (i < n && iv[i][1] < ni[0]) res.add(iv[i++]);          // before
    while (i < n && iv[i][0] <= ni[1]) {                         // overlap -> merge
        ni[0] = Math.min(ni[0], iv[i][0]);
        ni[1] = Math.max(ni[1], iv[i][1]);
        i++;
    }
    res.add(ni);
    while (i < n) res.add(iv[i++]);                              // after
    return res.toArray(new int[0][]);
}
```

## Template — minimum meeting rooms (max concurrent overlap)
```java
int minRooms(int[][] iv){
    int n = iv.length; int[] start = new int[n], end = new int[n];
    for (int i = 0; i < n; i++){ start[i]=iv[i][0]; end[i]=iv[i][1]; }
    Arrays.sort(start); Arrays.sort(end);
    int rooms = 0, max = 0, s = 0, e = 0;
    while (s < n){
        if (start[s] < end[e]){ rooms++; s++; }                  // a meeting starts
        else { rooms--; e++; }                                   // one ended, free a room
        max = Math.max(max, rooms);
    }
    return max;
}
```
> [!tip] The "two-pointer sweep over sorted start/end arrays" counts concurrent overlap in O(n log n). A min-heap of end times is an alternative: push end, pop all ended before the next start, heap size = rooms.

## Variants
- **Non-overlapping count to remove**: sort by end, greedy pick compatible intervals (see [[Greedy]] activity selection) → `total − maxCompatible`.
- **Interval intersection** of two lists: two pointers, advance the one ending earlier, record overlaps.
- **Employee free time**: merge all, then report gaps between consecutive merged intervals.

## Complexity
- Time: O(n log n) (the sort dominates).
- Space: O(n) for the output (O(1) extra for in-place merge variants).

> [!warning] Pitfalls
  - Sorting by the **wrong key** (e.g. by end when merge needs start; by length never works). Merge → by start; activity-selection max count → by end.
  - Inclusive vs exclusive ends: decide if `[1,2]` and `[2,3]` overlap (often they don't — `end < start` means no overlap, so equal end/start = adjacent, not overlapping). Match the problem's definition.
  - Mutating the input array in `merge` (sort reorders it) — clone if the caller needs original order.
  - Meeting rooms: counting `rooms` with `start[s] < end[e]` assumes equal times don't free a room before starting — verify the tie-break rule for the problem.

## Practice questions
- [[Merge-Intervals]] → merge
- [[Insert-Interval]] → insert
- [[Meeting-Rooms]] → any overlap
- [[Meeting-Rooms-II]] → max overlap
- [[Non-overlapping-Intervals]] → greedy by end
- [[Employee-Free-Time]] → merge + gaps
- [[Interval-List-Intersections]] → two pointers

## Related
- [[Sorting]] · [[Greedy]] · [[Arrays]]
