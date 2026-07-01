---
type: concept
tags:
  - dsa/pattern/graph-bfs
difficulty: hard
pattern: "[[Graph-BFS]]"
related:
  - "[[Graph]]"
  - "[[Queue]]"
  - "[[Tree-BFS]]"
  - "[[Topological-Sort]]"
  - "[[Disjoint-Set]]"
aliases:
  - Graph BFS
  - BFS
  - Shortest path unweighted
---

# Graph BFS

> [!note] Definition
> Breadth-first search on a graph/grid: explore all nodes at distance d before any at distance d+1, using a **queue**. On an **unweighted** graph it computes shortest-path distances from a source in O(V+E). Also the engine for flood-fill, islands, multi-source spread, and "fewest moves" puzzles.

## When it applies (cues)
- "Shortest path / fewest steps / minimum moves" on an unweighted graph or grid.
- "Number of islands / surrounded regions / flood fill" on a grid.
- "Rotting oranges / walls and gates" — multi-source BFS from all initial sources.
- "Word ladder" — BFS over an implicit graph (words connected by 1-letter edits).
- "01 matrix" — distance to nearest 0.
- "Maze shortest path."

## Template — single-source BFS
```java
int bfs(List<List<Integer>> g, int src, int dst){
    int n = g.size(); int[] dist = new int[n]; Arrays.fill(dist, -1);
    Deque<Integer> q = new ArrayDeque<>();
    q.offer(src); dist[src] = 0;
    while (!q.isEmpty()){
        int u = q.poll();
        if (u == dst) return dist[u];
        for (int v : g.get(u)) if (dist[v] == -1){ dist[v] = dist[u] + 1; q.offer(v); }
    }
    return -1;
}
```

## Template — grid BFS with directions
```java
int[][] dirs = {{-1,0},{1,0},{0,-1},{0,1}};
int bfsGrid(int[][] grid, int sr, int sc){
    int m = grid.length, n = grid[0].length;
    int[][] dist = new int[m][n]; for (int[] r : dist) Arrays.fill(r, -1);
    Deque<int[]> q = new ArrayDeque<>();
    q.offer(new int[]{sr, sc}); dist[sr][sc] = 0;
    while (!q.isEmpty()){
        int[] u = q.poll(); int r = u[0], c = u[1];
        for (int[] d : dirs){
            int nr = r + d[0], nc = c + d[1];
            if (nr<0||nr>=m||nc<0||nc>=n) continue;
            if (grid[nr][nc] == 1 || dist[nr][nc] != -1) continue;   // blocked/visited
            dist[nr][nc] = dist[r][c] + 1; q.offer(new int[]{nr, nc});
        }
    }
    return -1;
}
```

## Template — multi-source BFS (rotting oranges, 01-matrix)
Push **all** sources into the queue at time 0, then BFS normally. The first time a cell is reached is its minimum distance to *any* source.
```java
// rotten oranges: enqueue all initially rotten, BFS to fresh, time = layers processed
Deque<int[]> q = new ArrayDeque<>();
for (each rotten cell) q.offer(cell);
int minutes = 0;
while (!q.isEmpty()){
    int size = q.size(); boolean spread = false;
    for (int i = 0; i < size; i++){
        int[] u = q.poll();
        for (each neighbor) if (fresh){ fresh-- ; mark rotten; q.offer(neighbor); spread = true; }
    }
    if (spread) minutes++;
}
return fresh == 0 ? minutes : -1;
```
> [!tip] Multi-source BFS = "run BFS from all sources at once." It computes, for each cell, the distance to the **nearest** source — exactly what "rotting oranges", "walls and gates", and "01 matrix" need.

## Template — number of islands (DFS or BFS flood fill)
```java
int numIslands(char[][] g){
    int m = g.length, n = g[0].length, count = 0;
    for (int i = 0; i < m; i++) for (int j = 0; j < n; j++)
        if (g[i][j] == '1'){ count++; floodBFS(g, i, j); }   // mark whole island
    return count;
}
void floodBFS(char[][] g, int sr, int sc){
    Deque<int[]> q = new ArrayDeque<>(); q.offer(new int[]{sr, sc}); g[sr][sc] = '0';
    while (!q.isEmpty()){
        int[] u = q.poll();
        for (int[] d : dirs){
            int r = u[0]+d[0], c = u[1]+d[1];
            if (r<0||r>=g.length||c<0||c>=g[0].length||g[r][c]!='1') continue;
            g[r][c] = '0'; q.offer(new int[]{r, c});
        }
    }
}
```

## Complexity
- Time: O(V + E) (grid: O(m·n)).
- Space: O(V) for the queue and dist array.

> [!warning] Pitfalls
  - **Mark visited on enqueue**, not dequeue — else a node is added many times (exponential blowup).
  - Forgetting to check bounds AND blocked AND unvisited in grid BFS.
  - Multi-source: enqueue all sources before the loop; don't run separate BFS per source (that's O(k·(V+E)) vs O(V+E)).
  - Word ladder: building the graph by comparing every pair is O(N²·L); build via wildcard patterns (`h*t` → neighbors) for O(N·L²).
  - 0-1 weights → use 0-1 BFS (deque, push front for 0-weight) or Dijkstra; plain BFS gives wrong distances.

## Practice questions
- [[Number-of-Islands]] → flood fill
- [[Rotting-Oranges]] → multi-source BFS
- [[Walls-and-Gates]] → multi-source (gates)
- [[01-Matrix]] → multi-source (zeros)
- [[Word-Ladder]] → implicit graph BFS
- [[Shortest-Path-in-Binary-Matrix]] → grid BFS
- [[Snakes-and-Ladders]] → BFS on a board graph

## Related
- [[Graph]] · [[Queue]] · [[Tree-BFS]] · [[Topological-Sort]] · [[Disjoint-Set]]
