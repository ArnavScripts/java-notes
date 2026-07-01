---
type: question
tags:
  - practice
  - hard
  - backtracking
difficulty: hard
pattern: "[[Backtracking]]"
source: "LeetCode 51"
related:
  - "[[Backtracking]]"
  - "[[Recursion]]"
  - "[[Sudoku-Solver]]"
aliases:
  - N-Queens
---

# N-Queens

> [!note] Problem
> Place `n` queens on an `n x n` chessboard so that no two attack each other. Return all distinct solutions (each as a board configuration).

## Examples
```
Input:  n = 4
Output: 2 solutions, e.g.
  .Q..        ..Q.
  ...Q        Q...
  Q...        ...Q
  ..Q.        .Q..
```

## Brute force
- Generate all placements of n queens in n² cells and filter → astronomical.

## Optimal approach
- Pattern: [[Backtracking]] with constraint pruning
- Idea: place queens row by row; for each row try every column, prune columns and both diagonals already attacked; recurse; undo.
- Steps:
  1. Track `col[j]`, diagonal `d1[r+c]`, anti-diagonal `d2[r-c+n]` as boolean attacked flags.
  2. `backtrack(row)`: if `row == n` → record; else for each column c not under attack, set the three flags, recurse `row+1`, clear them.

> [!tip] Diagonal encoding: cells on the same "/" diagonal share `r + c`; cells on the same "\" diagonal share `r - c` (offset by n to keep non-negative). This makes attacked-diagonal checks O(1).

## Complexity
- Time: O(n!) (pruned) — each row reduces valid columns.
- Space: O(n) for the flags + recursion.

## Java solution (count + build)
```java
public List<List<String>> solveNQueens(int n) {
    List<List<String>> out = new ArrayList<>();
    boolean[] col = new boolean[n], d1 = new boolean[2 * n], d2 = new boolean[2 * n];
    char[][] board = new char[n][n];
    for (char[] row : board) Arrays.fill(row, '.');
    backtrack(0, n, col, d1, d2, board, out);
    return out;
}
private void backtrack(int r, int n, boolean[] col, boolean[] d1, boolean[] d2,
                       char[][] board, List<List<String>> out) {
    if (r == n) {
        List<String> sol = new ArrayList<>();
        for (char[] row : board) sol.add(new String(row));
        out.add(sol); return;
    }
    for (int c = 0; c < n; c++) {
        if (col[c] || d1[r + c] || d2[r - c + n]) continue;        // prune
        board[r][c] = 'Q';
        col[c] = d1[r + c] = d2[r - c + n] = true;                 // make
        backtrack(r + 1, n, col, d1, d2, board, out);
        board[r][c] = '.';
        col[c] = d1[r + c] = d2[r - c + n] = false;                // undo
    }
}
```

> [!tip] The prune + make + recurse + undo quartet is the universal backtracking shape. Constraint arrays (instead of scanning the board for conflicts) turn an O(n) check into O(1) — the key to N-Queens being fast.

## Related
- [[Backtracking]] · [[Recursion]] · [[Sudoku-Solver]] · [[Word-Search]]
