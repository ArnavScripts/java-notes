---
type: concept
tags:
  - java/syntax
  - java/methods
difficulty: easy
pattern: ""
related:
  - "[[Variables-and-Data-Types]]"
  - "[[static-Keyword]]"
  - "[[Classes-and-Objects]]"
aliases:
  - Functions Java
  - Method Overloading
---

# Methods

> [!note] Definition
> A method is a named, reusable block of code with a signature `returnType name(params)`. Java is **strictly pass-by-value**: copies of primitives and copies of references are passed.

## Definition & call
```java
static int add(int a, int b) {
    return a + b;
}
// call
int s = add(3, 4);
```

## Pass-by-value explained
```java
void mutate(int x)        { x = 99; }                 // caller's int unchanged
void mutateArr(int[] a)   { a[0] = 99; }              // caller sees change (same object)
void reassign(int[] a)    { a = new int[]{7}; }       // caller's ref unchanged
```
- Primitives: the value is copied → changes don't escape.
- References: the reference value (address) is copied → you can mutate the shared object, but reassigning the parameter doesn't affect the caller.

## `varargs`
```java
static int sum(int... nums) {            // nums is int[]
    int total = 0; for (int v : nums) total += v; return total;
}
sum(1, 2, 3, 4);   // 10
```

## Overloading
Same name, different parameter list (count/type/order). Return type alone is **not** enough.
```java
int  max(int a, int b)       { return a > b ? a : b; }
long max(long a, long b)     { return a > b ? a : b; }
```

## Recursion base
```java
static int fact(int n) { return n <= 1 ? 1 : n * fact(n - 1); }
```
> [!tip] See [[Recursion]] for the full treatment — base case, call stack, recursion tree.

> [!warning] Pitfalls
> - Forgetting `return` on a non-void path → compile error.
> - Confusing pass-by-value with pass-by-reference (Java has no ref params).
> - Overload ambiguity: `max(5, 5L)` may need an explicit cast.

## Pattern recognition cues
- Repeated logic with varying inputs → extract a method.
- Same operation on different types → overload (or [[Generics]]).

## Related
- [[static-Keyword]] · [[Classes-and-Objects]] · [[Recursion]] · [[Generics]]
