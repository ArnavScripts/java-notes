---
type: concept
tags:
  - java/ds/tree
  - java/ds/bst
difficulty: hard
pattern: ""
related:
  - "[[Tree-BFS]]"
  - "[[Tree-DFS]]"
  - "[[Heap]]"
  - "[[Recursion]]"
  - "[[Divide-and-Conquer]]"
  - "[[DP]]"
aliases:
  - BST
  - Binary Search Tree
  - Binary Tree
---

# Binary Tree & BST

> [!note] Definition
> A **binary tree** = each node has up to 2 children (`left`, `right`). A **BST** is a binary tree with the ordering invariant: for each node, all keys in the left subtree are < node < all keys in the right subtree. BST ops are O(h); balanced (h = log n), skewed (h = n).

## Node model
```java
static class TreeNode {
    int val; TreeNode left, right;
    TreeNode(int v){ val = v; }
}
```

## Traversals (recursive)
```java
void pre(TreeNode r)  { if (r==null) return; visit(r); pre(r.left);  pre(r.right); }
void in(TreeNode r)   { if (r==null) return; in(r.left);  visit(r);  in(r.right); }  // sorted for BST!
void post(TreeNode r) { if (r==null) return; post(r.left); post(r.right); visit(r); }
```
> [!tip] **In-order traversal of a BST yields sorted order** — the defining property, used in k-th smallest, successor, validation.

## Traversals (iterative) → see [[Tree-DFS]]
- Level-order → queue → [[Tree-BFS]].
- Pre/in/post via explicit `ArrayDeque` stack.

## BST operations
```java
TreeNode search(TreeNode r, int key){
    while (r != null) r = key < r.val ? r.left : key > r.val ? r.right : r;
    return r;
}
TreeNode insert(TreeNode r, int key){
    if (r == null) return new TreeNode(key);
    if (key < r.val) r.left  = insert(r.left, key);
    else if (key > r.val) r.right = insert(r.right, key);
    return r;
}
```
Deletion is the tricky one: node with two children → replace with inorder successor (min of right subtree), then delete that successor.

## Validation (is BST)
```java
boolean isBST(TreeNode r){ return isBST(r, Long.MIN_VALUE, Long.MAX_VALUE); }
boolean isBST(TreeNode r, long lo, long hi){
    if (r == null) return true;
    return r.val > lo && r.val < hi
        && isBST(r.left, lo, r.val) && isBST(r.right, r.val, hi);
}
```

## Common derived metrics
- Height: `1 + max(height(left), height(right))` (DFS).
- Diameter: longest path between any two nodes — `max(leftH + rightH)` updated during height DFS.
- Balanced: both subtrees balanced AND height diff ≤ 1.
- LCA (lowest common ancestor): recurse; if current lies between the two nodes, it's the LCA (BST) / general recursion (binary tree).

> [!tip] Pattern recognition cues
> - "k-th smallest in BST" → in-order traversal, stop at k.
> - "validate BST" → bounded-range DFS.
> - "level by level" → [[Tree-BFS]].
  - "max depth / diameter / balanced / path sums" → [[Tree-DFS]] + recursion returning a value.
  - "all root-to-leaf paths / subsets of choices" → [[Backtracking]] / [[Subsets]].
  - "construct tree from inorder+preorder" → recursion with index ranges ([[Divide-and-Conquer]]).

> [!warning] Pitfalls
  - Skewed insert order → O(n) BST; use a self-balancing tree (`TreeMap`/`TreeSet`, AVL, Red-Black) in real code.
  - In-order printing only sorts if the tree is **actually a BST**; don't assume it for a plain binary tree.
  - Use `Long` bounds in validation to handle `Integer.MIN_VALUE/MAX_VALUE` node values.
  - Recursion depth = tree height → stack overflow on skewed trees ([[JVM-and-Memory]]); iterate if needed.

## Practice questions
- [[Maximum-Depth-of-Binary-Tree]] → [[Tree-DFS]]
- [[Validate-Binary-Search-Tree]] → bounded DFS
- [[Binary-Tree-Level-Order-Traversal]] → [[Tree-BFS]]
- [[Kth-Smallest-Element-in-a-BST]] → in-order
- [[Lowest-Common-Ancestor-of-a-Binary-Tree]] → recursion
- [[Diameter-of-Binary-Tree]] → DFS with max

## Related
- [[Tree-BFS]] · [[Tree-DFS]] · [[Heap]] (a binary heap is a complete binary tree) · [[Recursion]] · [[Divide-and-Conquer]]
