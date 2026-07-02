---
type: concept
tags:
  - dsa/ds
  - java/ds/graph
difficulty: hard
status: evergreen
related:
  - "[[Graph-BFS]]"
  - "[[Topological-Sort]]"
  - "[[Tree-DFS]]"
  - "[[Disjoint-Set]]"
  - "[[Heap]]"
  - "[[Recursion]]"
  - "[[Backtracking]]"
aliases:
  - Graph
  - Adjacency list
  - Adjacency matrix
---


# Graph

> [!note] Definition
> A graph G = (V, E): vertices connected by (optionally weighted/directed) edges. Representation choices drive complexity:
> - **Adjacency list** — `Map<V, List<V>>` or `List<Integer>[]`: O(V+E) space, default choice.
> - **Adjacency matrix** — `int[][]`: O(V²) space, O(1) edge query, good for dense graphs.
> - **Edge list** — `List<int[]>`: O(E), good for Kruskal / Bellman-Ford.

## Building an adjacency list (unweighted)
```java
int n = 5;
List<List<Integer>> g = new ArrayList<>();
for (int i = 0; i < n; i++) g.add(new ArrayList<>());
g.get(0).add(1); g.get(1).add(0);   // undirected edge 0-1
```
Weighted: store `int[]{to, weight}` or a `class Edge`.

## Directions & cycles
- **Directed**: edge `u→v` only. Cycle detection: DFS with recursion-stack coloring (white/gray/black) or Kahn's algo (in-degree zeroing).
- **Undirected**: edge both ways; cycle iff a visited non-parent neighbor is found during DFS.
- **DAG**: directed + acyclic → supports [[Topological-Sort]].

## Traversals
- **BFS** (queue): shortest path in **unweighted** graphs, level order → [[Graph-BFS]].
- **DFS** (stack/recursion): connectivity, cycle detection, topo sort, SCC, flood fill → [[Tree-DFS]] (same idea on graphs).

## Classic algorithms map
| Problem | Algorithm | Link |
|---------|-----------|------|
| Shortest path (unweighted) | BFS | [[Graph-BFS]] |
| Shortest path (weighted, ≥0) | Dijkstra + [[Heap]] | — |
| Shortest path (neg weights) | Bellman-Ford | — |
| All-pairs | Floyd-Warshall O(V³) | — |
| Min spanning tree | Kruskal ([[Disjoint-Set]]) / Prim ([[Heap]]) | — |
| Topological order (DAG) | DFS post-order / Kahn | [[Topological-Sort]] |
| Connected components | Union-Find / DFS | [[Disjoint-Set]] |
| Cycle detection | DFS coloring / Union-Find | — |
| Strongly connected | Kosaraju / Tarjan | — |
| Bipartite | BFS/DFS 2-coloring | — |
| Grid "islands"/flood fill | DFS/BFS on cells | [[Graph-BFS]] |

## Worked: BFS shortest path (unweighted)
```java
int bfs(List<List<Integer>> g, int s, int t){
    int n = g.size(); int[] d = new int[n]; Arrays.fill(d,-1);
    Deque<Integer> q = new ArrayDeque<>(); q.offer(s); d[s]=0;
    while(!q.isEmpty()){
        int u=q.poll();
        if(u==t) return d[u];
        for(int v:g.get(u)) if(d[v]==-1){ d[v]=d[u]+1; q.offer(v); }
    }
    return -1;
}
```

> [!tip] Pattern recognition cues
> - "Shortest path / fewest moves" → BFS if unweighted, Dijkstra if weighted non-negative.
> - "All prerequisites / ordering with deps" → [[Topological-Sort]].
> - "Are these connected / merge groups / detect cycle in undirected" → [[Disjoint-Set]].
> - "Grid moves / islands / flood fill" → treat cells as a graph; BFS/DFS.
> - "Path enumeration / hamiltonian / sudoku-like" → [[Backtracking]] DFS.
  - "Two-coloring / no odd cycle" → bipartite check.

> [!warning] Pitfalls
  - **Mark visited on enqueue, not dequeue**, or you'll add a node many times → TLE.
  - For undirected DFS, skip the **parent** when checking for back edges, else false cycles.
  - Dijkstra with negative edges fails; use Bellman-Ford or SPFA.
  - Adjacency matrix is O(V²) space — fine for V ≤ ~2000, wasteful otherwise.
  - Recursion DFS on V = 10^5 may overflow the stack; use iterative DFS or raise `-Xss`.

## Practice questions
- [[Number-of-Islands]] → DFS/BFS on grid
- [[Clone-Graph]] → DFS/BFS with map
- [[Course-Schedule]] → [[Topological-Sort]] / cycle detect
- [[Rotting-Oranges]] → multi-source BFS
- [[Network-Delay-Time]] → Dijkstra

## Related
- [[Graph-BFS]] · [[Topological-Sort]] · [[Tree-DFS]] · [[Disjoint-Set]] · [[Heap]] · [[Recursion]] · [[Backtracking]]
