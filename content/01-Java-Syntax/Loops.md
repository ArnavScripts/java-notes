---
type: concept
tags:
  - java/syntax
  - java/loops
difficulty: easy
pattern: ""
related:
  - "[[Arrays-Basics]]"
  - "[[Control-Flow]]"
  - "[[Recursion]]"
aliases: []
---

# Loops

> [!note] Definition
> Loops repeat a block. Choose by **known count** (for), **unknown count with a condition** (while), or **at-least-once** (do-while).

## The four loops
```java
for (int i = 0; i < n; i++) { ... }   // count-controlled
while (cond)            { ... }        // pre-test condition
do { ... } while (cond);               // post-test: runs >=1 time
for (int x : arr)       { ... }        // enhanced-for (no index)
```

## `break` vs `continue`
```java
for (int i = 0; i < n; i++) {
    if (arr[i] < 0) continue;   // skip rest of this iteration
    if (arr[i] == target) break; // exit loop entirely
}
```
Labeled break (rare but useful for nested loops / matrix search):
```java
outer:
for (int i = 0; i < n; i++)
    for (int j = 0; j < m; j++)
        if (matrix[i][j] == target) { System.out.println(i+","+j); break outer; }
```

## Infinite loop idiom
```java
while (true) {
    // ...use break to exit
}
```

> [!tip] Pattern recognition cues
> - "Process all elements" → enhanced-for (no index needed) or `for i`.
> - "Two-pointer convergence" → `while (left < right)` — see [[Two-Pointers]].
> - "Expand window" → `for (right = 0; right < n; right++)` with inner `while` to shrink — see [[Sliding-Window]].
> - "Same structure, smaller input" → consider [[Recursion]] instead of a loop.

> [!warning] Pitfalls
> - Off-by-one: `<` vs `<=` on the bound. Decide inclusive/exclusive up front.
> - Enhanced-for can't modify the array via the loop variable (it's a copy of the value) and gives no index.
> - `while (i < n);` with a stray `;` is an empty-body infinite loop.

## Practice questions
- FizzBuzz (1..100, multiples of 3/5)
- Sum of digits of an integer (use `while (n>0)`)

## Related
- [[Arrays-Basics]] · [[Recursion]] · [[Two-Pointers]] · [[Sliding-Window]]
