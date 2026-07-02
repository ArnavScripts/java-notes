---
type: moc
tags:
  - moc
  - dsa/algo
status: evergreen
related:
  - "[[00-Index-MOC]]"
  - "[[04-Data-Structures-MOC]]"
  - "[[06-Patterns-MOC]]"
  - "[[07-Practice-MOC]]"
aliases:
  - Algorithms MOC
---


# Algorithms — MOC

The "how" — procedures that transform inputs into answers, analyzed by **time/space complexity**. Patterns (folder 06) are reusable templates built from these primitives.

## Notes
- [[Complexity-Analysis]] — Big-O, recurrence solving, amortized cost
- [[Searching]] — linear, binary search, search space reduction
- [[Sorting]] — comparison sorts, comparators, non-comparison (counting/radix)
- [[Recursion]] — base case, call stack, recursion tree
- [[Backtracking]] — decision space + undo
- [[Divide-and-Conquer]] — split, solve, combine
- [[Greedy]] — locally optimal ⇒ globally optimal (when provable)
- [[DP]] — overlapping subproblems + optimal substructure
- [[Bit-Manipulation]] — XOR, masks, two's-complement tricks
- [[Math]] — number theory, GCD, primes, combinatorics, geometry

> [!tip] Mental model
> Algorithms = the steps; [[06-Patterns-MOC|Patterns]] = the *recognizable shapes* of those steps for common problem families. Master algorithms, then drill patterns.

## Dataview: this section
```dataview
TABLE difficulty, status, related as "Links"
WHERE file.folder = "05-Algorithms" AND type = "concept"
SORT file.name
```

## Dataview: thin notes to expand
```dataview
TABLE file.name as "Note", length(file.content) as "Chars"
WHERE file.folder = "05-Algorithms" AND type = "concept" AND length(file.content) < 3000
SORT length(file.content) ASC
```
