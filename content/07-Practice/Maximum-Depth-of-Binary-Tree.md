---
type: question
tags:
  - dsa/ds/tree
  - dsa/pattern/tree-dfs
  - practice
  - practice/easy
difficulty: easy
pattern: "[[Tree-DFS]]"
status: evergreen
source: "LeetCode 104"
related:
  - "[[Tree-DFS]]"
  - "[[BST]]"
  - "[[Recursion]]"
aliases:
  - Maximum Depth of Binary Tree
  - Max Depth
---


# Maximum Depth of Binary Tree

> [!note] Problem
> Given the `root` of a binary tree, return its maximum depth (number of nodes along the longest root-to-leaf path).

## Examples
```
Input:     3
          / \
         9  20
            / \
           15  7
Output: 3
```

## Brute force
- None needed; this *is* the base case.

## Optimal approach
- Pattern: [[Tree-DFS]] (aggregate returned up the recursion)
- Idea: depth(node) = 1 + max(depth(left), depth(right)); base case `null → 0`.
- Steps:
  1. If `root == null` return 0.
  2. Recurse left and right; return `1 + max(leftDepth, rightDepth)`.

## Complexity
- Time: O(n) · Space: O(h) recursion stack (h = height; log n balanced, n skewed)

## Java solution
```java
public int maxDepth(TreeNode root) {
    if (root == null) return 0;
    return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
}
```

## BFS alternative (level count)
```java
public int maxDepth(TreeNode root) {
    if (root == null) return 0;
    Deque<TreeNode> q = new ArrayDeque<>(); q.offer(root);
    int depth = 0;
    while (!q.isEmpty()) {
        depth++;
        for (int sz = q.size(); sz > 0; sz--) {
            TreeNode n = q.poll();
            if (n.left  != null) q.offer(n.left);
            if (n.right != null) q.offer(n.right);
        }
    }
    return depth;
}
```

> [!tip] The recursive form is the seed of many "aggregate-return" tree problems: [[Diameter-of-Binary-Tree]], [[Balanced-Binary-Tree]], [[Binary-Tree-Maximum-Path-Sum]] all reuse this skeleton.

## Related
- [[Tree-DFS]] · [[BST]] · [[Recursion]] · [[Minimum-Depth-of-Binary-Tree]]
