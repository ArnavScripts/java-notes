---
type: concept
tags:
  - java/ds/stack
difficulty: easy
pattern: ""
related:
  - "[[Queue]]"
  - "[[Monotonic-Stack]]"
  - "[[Recursion]]"
  - "[[Backtracking]]"
  - "[[Tree-DFS]]"
aliases:
  - Stack DS
  - LIFO
---

# Stack

> [!note] Definition
> A **LIFO** container: last-in, first-out. Push/pop/peek all O(1). Models "undo", "most recent", balanced delimiters, call frames, DFS state.

## Java choices
```java
Deque<Integer> st = new ArrayDeque<>();   // preferred
st.push(1); st.pop(); st.peek(); st.isEmpty();
```
> [!warning] Don't use legacy `java.util.Stack` — it's synchronized and extends `Vector`. `ArrayDeque` is faster and cleaner.

## Complexity
All core ops O(1); search O(n); space O(n).

## Classic uses
- **Balanced brackets**: push openers, pop on closers, check match and empty at end.
- **Postfix evaluation / infix→postfix**: two-stack Dijkstra shunting yard.
- **DFS** (explicit stack): push nodes, pop, process, push neighbors. See [[Tree-DFS]] / [[Graph-BFS]].
- **Undo / backtracking state**: push choice, pop on backtrack ([[Backtracking]]).
- **Next greater element** → [[Monotonic-Stack]].
- **Min stack**: keep a parallel stack of running minima for O(1) `getMin`.

## Worked: balanced brackets
```java
boolean valid(String s) {
    Deque<Character> st = new ArrayDeque<>();
    for (char c : s.toCharArray()) {
        if ("([{".indexOf(c) >= 0) st.push(c);
        else {
            if (st.isEmpty()) return false;
            char o = st.pop();
            if ((c==')'&&o!='(') || (c==']'&&o!='[') || (c=='}'&&o!='{')) return false;
        }
    }
    return st.isEmpty();
}
```

## Worked: min stack (two stacks)
```java
Deque<Integer> vals = new ArrayDeque<>(), mins = new ArrayDeque<>();
void push(int x){ vals.push(x); if (mins.isEmpty()||x<=mins.peek()) mins.push(x); }
int pop(){ int v=vals.pop(); if (v==mins.peek()) mins.pop(); return v; }
int getMin(){ return mins.peek(); }
```

> [!tip] Pattern recognition cues
> - "Next greater/smaller element", "largest rectangle in histogram", "stock span" → [[Monotonic-Stack]].
> - "Process nested/matching structure", "validate sequence" → plain stack.
> - "DFS without recursion" → explicit stack.
> - "Min/max under operations" → auxiliary stack or [[Heap]] if arbitrary order.

> [!warning] Pitfalls
> - `peek()`/`pop()` on empty → exception; guard with `isEmpty()`.
> - Confusing `addFirst/removeFirst` with `push/pop`: `push` == `addFirst`, `pop` == `removeFirst` (head end).
> - Stack size can balloon in deep DFS → watch memory / recursion→iteration trade-off.

## Practice questions
- [[Valid-Parentheses]] → stack
- [[Min-Stack]] → auxiliary stack
- [[Next-Greater-Element]] → [[Monotonic-Stack]]
- [[Evaluate-Reverse-Polish-Notation]] → stack
- [[Largest-Rectangle-in-Histogram]] → [[Monotonic-Stack]]

## Related
- [[Queue]] · [[Monotonic-Stack]] · [[Recursion]] (call stack) · [[Backtracking]] · [[Tree-DFS]]
