---
type: question
tags:
  - practice
  - medium
  - graph
  - topologicalsort
difficulty: medium
pattern: "[[Topological-Sort]]"
source: "LeetCode 207"
related:
  - "[[Topological-Sort]]"
  - "[[Graph]]"
  - "[[Course-Schedule-II]]"
aliases:
  - Course Schedule
---

# Course Schedule

> [!note] Problem
> There are `numCourses` labeled `0..n-1` and prerequisites `prerequisites[i] = [a, b]` meaning you must take `b` before `a`. Return `true` if you can finish all courses (i.e. the prerequisite graph has **no cycle**).

## Examples
```
Input:  numCourses = 2, prerequisites = [[1,0]]
Output: true            // 0 -> 1, no cycle

Input:  numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false           // 0 <-> 1, cycle
```

## Brute force
- Detect a cycle via DFS for each start → redundant; one DFS with colors is enough.

## Optimal approach
- Pattern: [[Topological-Sort]] (cycle detection)
- Idea: build the directed graph; run Kahn's (in-degree BFS) — if the produced order contains all n nodes, it's acyclic.
- Steps:
  1. Build adjacency list + in-degree array from `prerequisites` (edge `b -> a`).
  2. Enqueue all in-degree-0 nodes; BFS, decrementing neighbors' in-degrees.
  3. Return `completed == numCourses`.

## Complexity
- Time: O(V + E) · Space: O(V + E)

## Java solution (Kahn)
```java
public boolean canFinish(int numCourses, int[][] prerequisites) {
    List<List<Integer>> g = new ArrayList<>();
    int[] indeg = new int[numCourses];
    for (int i = 0; i < numCourses; i++) g.add(new ArrayList<>());
    for (int[] p : prerequisites) { g.get(p[1]).add(p[0]); indeg[p[0]]++; }
    Deque<Integer> q = new ArrayDeque<>();
    for (int i = 0; i < numCourses; i++) if (indeg[i] == 0) q.offer(i);
    int done = 0;
    while (!q.isEmpty()) {
        int u = q.poll(); done++;
        for (int v : g.get(u)) if (--indeg[v] == 0) q.offer(v);
    }
    return done == numCourses;
}
```

## Alternative: three-color DFS
`state[u] = 0/1/2` (white/gray/black). On entering a gray node → back edge → cycle. See [[Topological-Sort]].

## Related
- [[Topological-Sort]] · [[Graph]] · [[Course-Schedule-II]] (produce the order) · [[Parallel-Courses]]
