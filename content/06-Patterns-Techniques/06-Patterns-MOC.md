---
type: moc
tags:
  - moc
  - dsa/pattern
related:
  - "[[00-Index-MOC]]"
  - "[[05-Algorithms-MOC]]"
  - "[[04-Data-Structures-MOC]]"
  - "[[07-Practice-MOC]]"
aliases:
  - Patterns MOC
  - Techniques MOC
---

# Patterns & Techniques — MOC

The **pattern-recognition core** of DSA. Most interview/contest problems are instances of a handful of reusable templates. Learn the smell of each, and a "new" problem becomes a known one.

> [!tip] How to study patterns
> For each: read the **cues** (when it applies), study the **template**, solve the linked questions, then ask on every new problem *"which pattern does this smell like?"* Many problems combine 2+ patterns.

## Patterns
- [[Two-Pointers]] — two indices converge / scan
- [[Sliding-Window]] — contiguous subarray/substring with constraint
- [[Fast-Slow-Pointers]] — Floyd, cycle/middle on lists
- [[Merge-Intervals]] — overlapping ranges
- [[Cyclic-Sort]] — place 1..n at their indices
- [[In-Place-Reversal]] — reverse list/array portions
- [[Tree-BFS]] — level-order traversal
- [[Tree-DFS]] — preorder/inorder/postorder + path logic
- [[Subsets]] — power set / combinations via choose-unchoose
- [[Graph-BFS]] — shortest path on unweighted graphs/grids
- [[Topological-Sort]] — order with dependencies (DAG)
- [[K-Way-Merge]] — merge k sorted streams via heap
- [[Monotonic-Stack]] — next greater/smaller, spans
- [[Prefix-Sum]] — O(1) range sums / count diffs

## Smell → pattern quick map
| Problem cue | Pattern |
|-------------|---------|
| "sorted array, pair/triplet sum" | [[Two-Pointers]] |
| "longest/shortest contiguous subarray with constraint" | [[Sliding-Window]] |
| "cycle in list / find middle / happy number" | [[Fast-Slow-Pointers]] |
| "overlapping meetings / insert interval" | [[Merge-Intervals]] |
| "array of 1..n, missing/duplicate" | [[Cyclic-Sort]] |
| "reverse every k nodes / reverse sublist" | [[In-Place-Reversal]] |
| "level by level / right side view" | [[Tree-BFS]] |
| "max depth / path sum / validate" | [[Tree-DFS]] |
| "all subsets / combinations" | [[Subsets]] |
| "shortest path / fewest moves / islands" | [[Graph-BFS]] |
| "prerequisites / build order / course schedule" | [[Topological-Sort]] |
| "merge k sorted lists/arrays" | [[K-Way-Merge]] |
| "next greater element / stock span / largest rectangle" | [[Monotonic-Stack]] |
| "sum of subarray [l..r] / count subarrays with sum k" | [[Prefix-Sum]] |

## Dataview: all pattern notes
```dataview
TABLE difficulty, related as "Links"
WHERE file.folder = "06-Patterns-Techniques" AND type = "concept"
SORT file.name
```

## Dataview: questions grouped by pattern
```dataview
TABLE file.name as "Question", difficulty
WHERE type = "question"
GROUP BY pattern
SORT pattern
```
