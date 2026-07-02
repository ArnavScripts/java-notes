---
type: concept
tags:
  - java/oops
  - java/object
difficulty: medium
status: evergreen
related:
  - "[[Classes-and-Objects]]"
  - "[[Inheritance]]"
  - "[[Polymorphism]]"
  - "[[Encapsulation]]"
  - "[[HashMap]]"
aliases:
  - Object class
  - equals hashCode
  - toString
---


# The `Object` Class

> [!note] Definition
> `java.lang.Object` is the implicit superclass of every class. Its methods are inherited by all objects and are the **contract** you override most often. Getting `equals`/`hashCode` right is critical for collections ([[HashMap]], [[HashSet]]).

## The key methods
```java
String  toString()              // human-readable representation
boolean equals(Object o)        // logical equality
int     hashCode()              // int for hash-based storage
Class<?> getClass()             // runtime type (reflection)
Object  clone()                 // shallow copy (Cloneable marker)
void    finalize()              // deprecated; avoid
```
Plus `wait/notify/notifyAll` for [[Multithreading]].

## `toString()`
Default returns `ClassName@hexHash`. Override for readability and debugging.
```java
@Override public String toString() {
    return "Point(" + x + "," + y + ")";
}
```

## `equals()` contract
1. **Reflexive**: `a.equals(a)` true.
2. **Symmetric**: `a.equals(b)` ⇒ `b.equals(a)`.
3. **Transitive**: `a.equals(b)` and `b.equals(c)` ⇒ `a.equals(c)`.
4. **Consistent**: repeated calls stay equal (no mutation).
5. `a.equals(null)` is `false`.
```java
@Override public boolean equals(Object o) {
    if (this == o) return true;                  // same reference
    if (!(o instanceof Point)) return false;     // null-safe + type check
    Point p = (Point) o;
    return this.x == p.x && this.y == p.y;
}
```

## `hashCode()` contract — the vital pairing
1. Equal objects **must** have equal hash codes.
2. Unequal objects *ideally* have different hash codes (for performance).
3. Consistent across calls (no mutation).

> [!warning] Break `equals`/`hashCode` consistency and `HashMap`/`HashSet` lose your objects. If `a.equals(b)` then `a.hashCode() == b.hashCode()` is **mandatory**. The reverse is not required but dramatically improves bucket distribution.

## Canonical `hashCode` (Objects utility)
```java
@Override public int hashCode() {
    return Objects.hash(x, y);
}
```

> [!tip] Pattern recognition
> Custom object as a `HashMap` key or `HashSet` element? → you **must** override `equals` and `hashCode` (or make it a record/`record` which auto-generates them). Forgetting this is the #1 cause of "why isn't my key found?" bugs.

## `instanceof` vs `getClass()` for equality
- `instanceof` (pattern: `o instanceof Point p`) → allows subclasses to be equal (Liskov-friendly). Preferred in most cases.
- `getClass()` comparison → exact-type equality (used by some libraries; breaks symmetry with subclasses).

## Related
- [[Classes-and-Objects]] · [[Inheritance]] · [[Polymorphism]] · [[HashMap]] · [[HashSet]] · [[Multithreading]]
