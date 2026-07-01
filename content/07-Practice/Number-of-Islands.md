---
type: question
tags:
  - practice
  - medium
  - graph
  - bfs
  - grid
difficulty: medium
pattern: "[[Graph-BFS]]"
source: "LeetCode 200"
related:
  - "[[Graph-BFS]]"
  - "[[Graph]]"
  - "[[Disjoint-Set]]"
aliases:
  - Number of Islands
---

# Number of Islands

> [!note] Problem
> Given an `m x n` grid of `'1'` (land) and `'0'` (water), count the number of islands (4-directionally connected land cells).

## Examples
```
Grid:
  1 1 0 0 0
  1 1 0 0 0
  0 0 1 0 0
  0 0 0 1 1
Output: 3
```

## Brute force
- None meaningful; this is the canonical flood-fill.

## Optimal approach
- Pattern: [[Graph-BFS]] (or DFS) flood fill — also solvable with [[Disjoint-Set]]
- Idea: scan every cell; on finding unvisited land, increment count and flood-fill (BFS/DFS) the entire island marking cells visited (or sink them to `'0'`).
- Steps:
  1. For each `(i,j)` with `grid[i][j] == '1'`: count++; BFS/DFS to mark all connected `'1'`s as `'0'`.
  2. Return count.

## Complexity
- Time: O(m·n) — each cell processed once.
- Space: O(m·n) worst case for the BFS queue / DFS stack (all land).

## Java solution (BFS flood fill)
```java
public int numIslands(char[][] grid) {
    int m = grid.length, n = grid[0].length, count = 0;
    int[][] dirs = {{-1,0},{1,0},{0,-1},{0,1}};
    for (int i = 0; i < m; i++)
        for (int j = 0; j < n; j++)
            if (grid[i][j] == '1') {
                count++;
                Deque<int[]> q = new ArrayDeque<>();
                q.offer(new int[]{i, j}); grid[i][j] = '0';      // mark on enqueue
                while (!q.isEmpty()) {
                    int[] u = q.poll();
                    for (int[] d : dirs) {
                        int r = u[0] + d[0], c = u[1] + d[1];
                        if (r < 0 || r >= m || c < 0 || c >= n || grid[r][c] != '1') continue;
                        grid[r][c] = '0';                          // mark on enqueue
                        q.offer(new int[]{r, c});
                    }
                }
            }
    return count;
}
```

> [!tip] Mark visited **on enqueue** (not dequeue) so each land cell enters the queue at most once. Sinking to `'0'` uses the grid itself as the visited set → O(1) extra state (queue aside).

## Variants
- DFS flood fill (recursive, O(m·n) stack risk on large grids).
- [[Disjoint-Set]] union of adjacent land cells, then count distinct roots.
- Number of distinct islands (encode the shape with a path signature).

## Related
- [[Graph-BFS]] · [[Graph]] · [[Disjoint-Set]] · [[Rotting-Oranges]] · [[Max-Area-of-Island]]
