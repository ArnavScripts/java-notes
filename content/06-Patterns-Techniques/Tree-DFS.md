---
type: concept
tags:
  - dsa/pattern/tree-dfs
difficulty: hard
pattern: "[[Tree-DFS]]"
related:
  - "[[BST]]"
  - "[[Recursion]]"
  - "[[Tree-BFS]]"
  - "[[Backtracking]]"
  - "[[Divide-and-Conquer]]"
aliases:
  - Tree DFS
  - Preorder
  - Inorder
  - Postorder
---

# Tree DFS

> [!note] Definition
> Depth-first traversal of a tree: go as deep as possible before backtracking. Three orderings: **preorder** (visit, left, right), **inorder** (left, visit, right — sorted for a BST), **postorder** (left, right, visit). Implemented recursively (natural) or iteratively with an explicit stack.

## When it applies (cues)
- "Max/min depth, diameter, is balanced, path sum."
- "Validate BST" (inorder must be increasing, or bounded DFS).
- "All root-to-leaf paths", "path sum II" (collect paths).
- "Serialize / deserialize a tree" (preorder with markers).
- "LCA of two nodes", "k-th smallest in a BST" (inorder).
- "Flatten / mirror / invert a tree."
- "Subtree check / count nodes" (postorder aggregates).

## Three recursive orderings
```java
void pre(TreeNode r){  if (r==null) return; visit(r); pre(r.left); pre(r.right); }
void in(TreeNode r){   if (r==null) return; in(r.left);  visit(r); in(r.right); }   // BST sorted
void post(TreeNode r){ if (r==null) return; post(r.left); post(r.right); visit(r); }
```

## Iterative preorder (stack)
```java
List<Integer> preorder(TreeNode root){
    List<Integer> res = new ArrayList<>();
    Deque<TreeNode> st = new ArrayDeque<>();
    if (root != null) st.push(root);
    while (!st.isEmpty()){
        TreeNode n = st.pop(); res.add(n.val);
        if (n.right != null) st.push(n.right);   // push right first so left is processed first
        if (n.left  != null) st.push(n.left);
    }
    return res;
}
```

## Iterative inorder (stack)
```java
List<Integer> inorder(TreeNode root){
    List<Integer> res = new ArrayList<>();
    Deque<TreeNode> st = new ArrayDeque<>();
    TreeNode cur = root;
    while (cur != null || !st.isEmpty()){
        while (cur != null){ st.push(cur); cur = cur.left; }   // go left
        cur = st.pop(); res.add(cur.val);                      // visit
        cur = cur.right;                                       // go right
    }
    return res;
}
```

## Returning aggregates up the recursion (the money pattern)
Many tree problems = "compute a value for the subtree and return it to the parent." Examples:
```java
int maxDepth(TreeNode r){ return r == null ? 0 : 1 + Math.max(maxDepth(r.left), maxDepth(r.right)); }

int diameter;                                   // updated as a side effect
int height(TreeNode r){                         // returns height; updates diameter
    if (r == null) return 0;
    int lh = height(r.left), rh = height(r.right);
    diameter = Math.max(diameter, lh + rh);     // path through this node
    return 1 + Math.max(lh, rh);
}
```
> [!tip] The "return one value, update a global/side value" idiom handles diameter, max path sum, is-balanced-with-height, etc. The return feeds the parent; the global tracks the cross-node answer.

## Path collection (root-to-leaf) — backtracking style
```java
void paths(TreeNode r, int sum, List<Integer> cur, List<List<Integer>> out){
    if (r == null) return;
    cur.add(r.val);
    if (r.left == null && r.right == null && sum == r.val) out.add(new ArrayList<>(cur));
    paths(r.left,  sum - r.val, cur, out);
    paths(r.right, sum - r.val, cur, out);
    cur.remove(cur.size()-1);                   // backtrack
}
```
> [!tip] This is [[Backtracking]] on a tree: choose a child, recurse, un-choose. Copy `cur` when recording a solution.

## Validate BST (bounded DFS)
```java
boolean isBST(TreeNode r){ return isBST(r, Long.MIN_VALUE, Long.MAX_VALUE); }
boolean isBST(TreeNode r, long lo, long hi){
    if (r == null) return true;
    return r.val > lo && r.val < hi
        && isBST(r.left, lo, r.val) && isBST(r.right, r.val, hi);
}
```

## LCA (binary tree)
```java
TreeNode lca(TreeNode r, TreeNode p, TreeNode q){
    if (r == null || r == p || r == q) return r;
    TreeNode l = lca(r.left, p, q), rr = lca(r.right, p, q);
    return l == null ? rr : rr == null ? l : r;   // both found -> r is LCA
}
```

## Complexity
- Time: O(n) for full traversal; O(h) for targeted BST ops (h = height).
- Space: O(h) recursion/stack (h = log n balanced, n skewed).

> [!warning] Pitfalls
  - Recursion depth = height; skewed trees → stack overflow ([[JVM-and-Memory]]) → iterate.
  - In-order sortedness **only** holds for actual BSTs; don't assume it for arbitrary binary trees.
  - Long bounds for BST validation (node values can be `Integer.MIN/MAX_VALUE`).
  - Collecting paths: record `new ArrayList<>(cur)` (a copy), not the live list — it keeps mutating.
  - Side-effect globals (diameter/path-sum) must be reset between test cases.

## Practice questions
- [[Maximum-Depth-of-Binary-Tree]] → height DFS
- [[Diameter-of-Binary-Tree]] → aggregate-return idiom
- [[Same-Tree]] / [[Symmetric-Tree]] → simultaneous DFS
- [[Path-Sum]] / [[Path-Sum-II]] → root-to-leaf DFS
- [[Validate-Binary-Search-Tree]] → bounded DFS
- [[Binary-Tree-Maximum-Path-Sum]] → aggregate-return
- [[Kth-Smallest-Element-in-a-BST]] → inorder
- [[Lowest-Common-Ancestor-of-a-Binary-Tree]] → recursion
- [[Serialize-and-Deserialize-Binary-Tree]] → preorder + markers

## Related
- [[BST]] · [[Recursion]] · [[Tree-BFS]] · [[Backtracking]] · [[Divide-and-Conquer]]
