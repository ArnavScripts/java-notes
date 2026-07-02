---
type: concept
tags:
  - dsa/ds
  - java/ds/dsu
difficulty: hard
status: evergreen
related:
  - "[[Graph]]"
  - "[[Topological-Sort]]"
  - "[[Recursion]]"
aliases:
  - Disjoint Set
  - Union-Find
  - DSU
---


# Disjoint Set (Union-Find)

> [!note] Definition
> A disjoint-set / union-find (DSU) maintains a partition of elements into sets, supporting:
> - `find(x)` — the representative of x's set.
> - `union(x, y)` — merge the sets containing x and y.
> - `connected(x, y)` — same set?
> With **path compression** + **union by rank/size**, every op is **α(n) ≈ O(1)** amortized (inverse Ackermann).

## Implementation
```java
static class DSU {
    int[] parent, rank;
    DSU(int n){ parent = new int[n]; rank = new int[n];
        for (int i = 0; i < n; i++) parent[i] = i; }
    int find(int x){
        if (parent[x] != x) parent[x] = find(parent[x]);   // path compression
        return parent[x];
    }
    boolean union(int a, int b){
        int ra = find(a), rb = find(b);
        if (ra == rb) return false;                 // already connected
        if (rank[ra] < rank[rb]) { int t=ra; ra=rb; rb=t; }
        parent[rb] = ra;
        if (rank[ra] == rank[rb]) rank[ra]++;
        return true;                                // a merge happened
    }
}
```

## Why it's fast
- Path compression flattens trees on each `find`.
- Union by rank keeps trees shallow (height ≤ log n).
- Together → amortized α(n), essentially constant.

## Counting components
```java
int components = n;
// each successful union (returned true) does:
if (union(a, b)) components--;
```
Start with `n` singleton sets; every real merge decrements.

## Cycle detection in undirected graphs
For each edge `(u, v)`: if `find(u) == find(v)` → cycle; else `union(u, v)`.

## Classic uses
- **Connected components** (undirected) — count after all unions.
- **Cycle detection** in undirected graphs.
- **Kruskal's MST** — sort edges by weight; union while no cycle; take V−1 edges.
- **Dynamic connectivity**, account merging, redundant connections.
- **Offline** "are these connected?" with no deletions.

## Worked: number of connected components
```java
int countComponents(int n, int[][] edges){
    DSU dsu = new DSU(n);
    int comp = n;
    for (int[] e : edges) if (dsu.union(e[0], e[1])) comp--;
    return comp;
}
```

## Worked: redundant connection (find cycle edge)
```java
int[] findRedundant(int[][] edges){
    DSU dsu = new DSU(edges.length + 1);
    for (int[] e : edges) if (!dsu.union(e[0], e[1])) return e;  // closes a cycle
    return null;
}
```

> [!tip] Pattern recognition cues
> - "Connected components / merge groups / are these in the same group?" → DSU.
> - "Find the redundant edge / earliest cycle edge" → DSU; first edge whose endpoints are already connected.
> - "MST with weights" → Kruskal + DSU.
> - "Offline queries of connectivity" → DSU (no deletions; for deletions use a reverse-rollback approach).
> - Bipartiteness / DAG ordering → **not** DSU (use 2-coloring / topo sort).

> [!warning] Pitfalls
  - Initialize `parent[i] = i` for every i; forgetting leaves all pointing to 0.
  - 0-based vs 1-based node labels — pick one and stay consistent.
  - DSU supports only **union** and **find**; it cannot split sets (no "undo" without rollback DSU).
  - Path compression + recursion on deep trees may overflow the stack; iterative `find` is safer for huge n.
  - For **directed** graphs use SCC algorithms, not DSU.

## Practice questions
- [[Number-of-Connected-Components]] → DSU
- [[Redundant-Connection]] → DSU cycle edge
- [[Accounts-Merge]] → DSU + map
- [[Graph-Valid-Tree]] → DSU (n-1 edges, no cycle, connected)
- [[Most-Stones-Removed]] → DSU on coordinates

## Related
- [[Graph]] · [[Topological-Sort]] · [[Recursion]]
