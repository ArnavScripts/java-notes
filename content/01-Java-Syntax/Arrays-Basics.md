---
type: concept
tags:
  - java/syntax
  - java/arrays
difficulty: easy
pattern: ""
related:
  - "[[Loops]]"
  - "[[Arrays]]"
  - "[[Methods]]"
aliases:
  - Java Arrays
---

# Arrays Basics

> [!note] Definition
> An array is a fixed-length, indexed, contiguous block storing one type. Length is immutable; elements default to `0`/`false`/`null`. Accessed in O(1) by index.

## Declaration & initialization
```java
int[] a = new int[5];          // [0,0,0,0,0], length 5
int[] b = {2, 4, 6, 8};        // length 4
int[] c = new int[]{1, 2, 3};  // explicit in expressions
String[] names = new String[3];// [null, null, null]
```

## Common operations
```java
a.length          // field, NOT a method (no parentheses)
a[i]              // get/set O(1)
Arrays.sort(a);   // in-place, O(n log n)
Arrays.fill(a, -1);
Arrays.copyOf(a, n);
Arrays.equals(a, b);
System.out.println(Arrays.toString(a));   // readable print
```

## 2D arrays
```java
int[][] grid = new int[3][4];          // 3 rows x 4 cols, all 0
int[][] jagged = {{1}, {2,3}, {4,5,6}};// rows may differ in length
int rows = grid.length, cols = grid[0].length;
```
Traversal:
```java
for (int i = 0; i < rows; i++)
    for (int j = 0; j < cols; j++)
        // grid[i][j]
```

> [!tip] DSA defaults
> - The array is the **canvas** for most patterns: [[Two-Pointers]], [[Sliding-Window]], [[Prefix-Sum]], [[Cyclic-Sort]], [[In-Place-Reversal]].
> - Sorting first unlocks [[Two-Pointers]] and binary search ([[Searching]]).
> - `int[]` return type for many solutions; `Arrays.stream(a).max()` for quick max.

> [!warning] Pitfalls
> - `a.length` has no `()`.
> - Index out of `[0, length-1]` → `ArrayIndexOutOfBoundsException`.
> - Comparing two arrays with `==` compares references; use `Arrays.equals`.
> - 2D `int[][] grid = new int[n][]` gives `null` rows — allocate each before use.

## Practice questions
- Reverse an array in place → [[In-Place-Reversal]]
- Find the max element
- Two-sum (sorted) → [[Two-Pointers]]

## Related
- [[Arrays]] (DS view) · [[Loops]] · [[Methods]]
