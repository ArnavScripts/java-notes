---
type: concept
tags:
  - dsa/pattern/topo-sort
difficulty: hard
pattern: "[[Topological-Sort]]"
related:
  - "[[Graph]]"
  - "[[Queue]]"
  - "[[Tree-DFS]]"
  - "[[Disjoint-Set]]"
aliases:
  - Topological Sort
  - Kahn's algorithm
  - Course schedule
---

# Topological Sort

> [!note] Definition
> A linear ordering of a **DAG**'s vertices such that for every edge `u → v`, u comes before v. Two algorithms: **Kahn's** (BFS, repeatedly remove in-degree-0 vertices) and **DFS post-order** (push vertices onto a stack after visiting all descendants). Detects cycles: if the order doesn't include all vertices, a cycle exists.

## When it applies (cues)
- "Course schedule / build order / prerequisites."
- "Compile order / package dependency order."
- "Alien dictionary" (derive ordering from sorted words → build DAG → topo sort).
- "Parallel course / minimum semesters" (layered topo via BFS levels).
- "Sequence reconstruction" (whether a unique topo order exists).
- "Find eventual safe states" (reverse graph + terminal nodes).

## Template A — Kahn's algorithm (BFS, in-degree)
```java
int[] topoKahn(int n, int[][] edges){
    List<List<Integer>> g = new ArrayList<>(); for (int i=0;i<n;i++) g.add(new ArrayList<>());
    int[] indeg = new int[n];
    for (int[] e : edges){ g.get(e[0]).add(e[1]); indeg[e[1]]++; }
    Deque<Integer> q = new ArrayDeque<>();
    for (int i = 0; i < n; i++) if (indeg[i] == 0) q.offer(i);
    int[] order = new int[n]; int idx = 0;
    while (!q.isEmpty()){
        int u = q.poll(); order[idx++] = u;
        for (int v : g.get(u)) if (--indeg[v] == 0) q.offer(v);
    }
    return idx == n ? order : null;        // null => cycle
}
```
> [!tip] Kahn's is also a **cycle detector**: if the produced order has fewer than n vertices, the remaining ones are on a cycle. This is the standard "course schedule" check.

## Template B — DFS post-order (reverse finish order)
```java
List<Integer> topoDFS(int n, List<List<Integer>> g){
    int[] state = new int[n];   // 0=unvisited, 1=visiting, 2=done
    LinkedList<Integer> order = new LinkedList<>();
    for (int i = 0; i < n; i++) if (state[i] == 0)
        if (!dfs(i, g, state, order)) return null;     // cycle -> null
    return order;                                       // already reversed (addFirst)
}
boolean dfs(int u, List<List<Integer>> g, int[] state, LinkedList<Integer> order){
    if (state[u] == 1) return false;     // back edge -> cycle
    if (state[u] == 2) return true;
    state[u] = 1;
    for (int v : g.get(u)) if (!dfs(v, g, state, order)) return false;
    state[u] = 2; order.addFirst(u);     // prepend => reverse finish order
    return true;
}
```
> [!tip] The three-color DFS (white/gray/black) detects cycles during topo sort. `state[u]==1` (gray) on re-entry = back edge = cycle.

## Worked — alien dictionary
Given sorted words, infer character precedence: compare adjacent words, the first differing pair gives an edge `a → b`. Build the DAG, topo sort, concatenate labels.
```java
// for each adjacent word pair, find first differing chars c1, c2 -> edge c1 -> c2
// then Kahn/DFS as above over the alphabet
```
> [!warning] Edge case: a prefix situation like `["abc","ab"]` is invalid (a longer word before its own prefix) → return "".

## Worked — parallel courses (min semesters)
Each topo "layer" (all in-degree-0 nodes at once) = one semester; count layers via BFS level size.
```java
int semesters = 0; ... while(!q.isEmpty()){ int size=q.size(); for(int i=0;i<size;i++){...} semesters++; }
```

## Complexity
- Time: O(V + E).
- Space: O(V + E) (graph + state/in-degree).

> [!warning] Pitfalls
  - Input may not be a DAG → always detect cycles (Kahn: `idx < n`; DFS: gray re-entry) and return the failure signal the problem wants.
  - Multiple valid topo orders exist; if the problem needs a *unique* one, compare against candidates or check that each BFS step dequeues exactly one node.
  - Kahn's: decrement in-degree only for actual edges; don't re-enqueue a node that's already in the queue.
  - DFS post-order: prepend (or reverse at the end) — appending gives the reverse of topo order.
  - Node labeling 0..n-1 vs arbitrary strings → map strings to ids first ([[HashMap]]).

## Practice questions
- [[Course-Schedule]] → cycle detect (Kahn)
- [[Course-Schedule-II]] → produce order
- [[Parallel-Courses]] / [[Parallel-Courses-III]] → layered BFS
- [[Alien-Dictionary]] → build DAG + topo
- [[Sequence-Reconstruction]] → unique topo check
- [[Find-Eventual-Safe-States]] → reverse graph / color DFS

## Related
- [[Graph]] · [[Queue]] · [[Tree-DFS]] · [[Disjoint-Set]]
