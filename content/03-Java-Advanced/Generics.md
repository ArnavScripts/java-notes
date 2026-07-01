---
type: concept
tags:
  - java/advanced
  - java/generics
difficulty: medium
pattern: ""
related:
  - "[[Collections-Framework]]"
  - "[[Abstraction-and-Interfaces]]"
  - "[[Methods]]"
aliases:
  - Generics
  - Type erasure
---

# Generics

> [!note] Definition
> Generics let you parameterize types: `List<String>`, `Pair<A,B>`, `Comparable<T>`. They give **compile-time** type safety (no casts, no `ClassCastException`) and code reuse across types.

## Generic class & method
```java
class Pair<A, B> {
    A first; B second;
    Pair(A a, B b){ first=a; second=b; }
    A key(){ return first; }
}

// generic method, independent of the class's type params
static <T extends Comparable<T>> T maxOf(List<T> xs) {
    T best = xs.get(0);
    for (T x : xs) if (x.compareTo(best) > 0) best = x;
    return best;
}
```

## Bounds — constrain the type parameter
```java
<T extends Number>            // upper bound: T is-a Number
<T extends Comparable<T>>     // require comparability (very common)
<T extends A & B & C>          // multiple bounds (at most one class, first)
```
No `super` on type parameters (that's only on wildcards).

## Wildcards
```java
void printAll(List<?> xs)                 // unknown type (read-only-ish)
void sum(List<? extends Number> xs)       // covariant: producer (read Number)
void addAll(List<? super Integer> xs)     // contravariant: consumer (write Integer)
```
**PECS** — *Producer Extends, Consumer Super*.

## Type erasure
Generics are mostly a compile-time feature: the compiler inserts casts and **erases** type parameters to their bound (`T`→`Object`, `T extends Number`→`Number`). Consequences:
- `new T()` is illegal — pass a `Supplier<T>` or `Class<T>` token.
- `new T[]` is illegal — use `(T[]) new Object[n]` (unchecked) or `Array.newInstance`.
- `List<String>.class == List<Integer>.class` (same erased class).
- `instanceof List<String>` illegal; use `instanceof List<?>`.

> [!tip] DSA usage
> - Write a `class TreeNode<T>` or `class Graph<T>` so your DS works for any type.
> - `<T extends Comparable<T>>` makes sort/comparator helpers generic.
> - `Comparator.comparingInt(Node::weight)` + generics = clean priority schedulers ([[Heap]]).

> [!warning] Pitfalls
> - Mixing raw and generic types gives "unchecked" warnings — don't ignore them; they signal heap pollution.
> - Can't overload methods that erase to the same signature `f(List<String>)` vs `f(List<Integer>)` — identical after erasure.
> - Primitive generics: `List<int>` is illegal → use `List<Integer>` (boxing cost) or a primitive array.

## Related
- [[Collections-Framework]] · [[Abstraction-and-Interfaces]] (`Comparable<T>`) · [[Methods]] (overloading)
