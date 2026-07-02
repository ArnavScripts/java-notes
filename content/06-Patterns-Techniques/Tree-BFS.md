---
type: concept
tags:
  - dsa/pattern
  - dsa/pattern/tree-bfs
difficulty: medium
pattern: "[[Tree-BFS]]"
status: evergreen
related:
  - "[[Queue]]"
  - "[[BST]]"
  - "[[Tree-DFS]]"
  - "[[Graph-BFS]]"
aliases:
  - Tree BFS
  - Level order traversal
---


# Tree BFS (Level-Order)

> [!note] Definition
> Traverse a tree **level by level** using a queue: process the root, enqueue its children, then repeatedly dequeue a node and enqueue its children. Captures "all nodes at distance d" in order — the natural fit for level-view, width, and "bottom-right" problems.

## When it applies (cues)
- "Level order traversal / print each level."
- "Right side view / left side view of a tree."
- "Average / max / min of each level."
- "Minimum depth / nearest leaf" (BFS finds shallowest first).
- "Maximum width of a tree" (level size / position indexing).
- "Connect next-right pointers" / "populating next right pointers."
- "Zigzag level order."

## Template — basic level order
```java
List<List<Integer>> levelOrder(TreeNode root){
    List<List<Integer>> res = new ArrayList<>();
    if (root == null) return res;
    Deque<TreeNode> q = new ArrayDeque<>(); q.offer(root);
    while (!q.isEmpty()){
        int size = q.size();                       // snapshot THIS level's count
        List<Integer> level = new ArrayList<>();
        for (int i = 0; i < size; i++){
            TreeNode n = q.poll();
            level.add(n.val);
            if (n.left  != null) q.offer(n.left);
            if (n.right != null) q.offer(n.right);
        }
        res.add(level);
    }
    return res;
}
```
> [!tip] The `size = q.size()` snapshot before the inner loop is the key: it lets you process exactly one level per outer iteration, separating levels into their own lists.

## Template — right side view
```java
List<Integer> rightSide(TreeNode root){
    List<Integer> res = new ArrayList<>();
    if (root == null) return res;
    Deque<TreeNode> q = new ArrayDeque<>(); q.offer(root);
    while (!q.isEmpty()){
        int size = q.size();
        for (int i = 0; i < size; i++){
            TreeNode n = q.poll();
            if (i == size - 1) res.add(n.val);     // last node of the level
            if (n.left  != null) q.offer(n.left);
            if (n.right != null) q.offer(n.right);
        }
    }
    return res;
}
```

## Template — zigzag (alternate direction per level)
```java
// same as level order, but reverse the level list on odd levels:
boolean ltr = true;
// inside the level loop:
if (!ltr) Collections.reverse(level);
res.add(level);
ltr = !ltr;
// or use a Deque and addFirst/addLast depending on direction (no reverse)
```

## Template — min depth (first leaf wins)
```java
int minDepth(TreeNode root){
    if (root == null) return 0;
    Deque<TreeNode> q = new ArrayDeque<>(); q.offer(root);
    int depth = 1;
    while (!q.isEmpty()){
        int size = q.size();
        for (int i = 0; i < size; i++){
            TreeNode n = q.poll();
            if (n.left == null && n.right == null) return depth;   // first leaf
            if (n.left  != null) q.offer(n.left);
            if (n.right != null) q.offer(n.right);
        }
        depth++;
    }
    return depth;
}
```
> [!tip] BFS finds the **shallowest** leaf first → O(n) worst case but often much faster than DFS for min depth. DFS min-depth would visit every node.

## Template — connect next-right pointers (constant space)
For a perfect/any binary tree, link each node's `next` to the next node on its right, using the already-established `next` of the parent level (O(1) space):
```java
Node connect(Node root){
    Node level = root;
    while (level != null){
        Node cur = level;
        while (cur != null){
            if (cur.left  != null) cur.left.next  = cur.right;
            if (cur.right != null) cur.right.next = cur.next == null ? null : cur.next.left;
            cur = cur.next;
        }
        level = level.left;
    }
    return root;
}
```

## Complexity
- Time: O(n) — each node enqueued/dequeued once.
- Space: O(w) where w = max level width (≤ n/2+1).

> [!warning] Pitfalls
  - Forgetting the `size` snapshot → all nodes collapse into one list (no level separation).
  - `poll()` returning null if the queue is empty — guard with `!isEmpty()` and the size loop.
  - Zigzag: reversing the list is O(w); a `Deque` with `addFirst`/`addLast` keeps it O(1) per node.
  - Min-depth via BFS returns the first leaf — make sure to check `left==null && right==null`, not just `left==null`.

## Practice questions
- [[Binary-Tree-Level-Order-Traversal]] → level order
- [[Binary-Tree-Right-Side-View]] → last per level
- [[Average-of-Levels-in-Binary-Tree]] → per-level sum/size
- [[Binary-Tree-Zigzag-Level-Order-Traversal]] → alternating
- [[Minimum-Depth-of-Binary-Tree]] → first leaf BFS
- [[Populating-Next-Right-Pointers]] → level linking
- [[Maximum-Width-of-Binary-Tree]] → indexed BFS

## Related
- [[Queue]] · [[BST]] · [[Tree-DFS]] · [[Graph-BFS]]
