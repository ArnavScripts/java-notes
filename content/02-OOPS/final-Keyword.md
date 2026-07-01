---
type: concept
tags:
  - java/oops
  - java/final
difficulty: easy
pattern: ""
related:
  - "[[Variables-and-Data-Types]]"
  - "[[Inheritance]]"
  - "[[Polymorphism]]"
aliases:
  - final keyword
---

# `final` Keyword

> [!note] Definition
> `final` means "this cannot be changed/redefined". Its meaning depends on **what** it decorates: a variable, a method, or a class.

## Three uses

**1. `final` variable** — assigned once, can't be reassigned
```java
final int MAX = 100;       // value constant
MAX = 200;                 // compile error

final Point p = new Point(1,2);
p = new Point(3,4);        // error: can't reassign reference
p.x = 99;                  // OK: the OBJECT is mutable; only the reference is final
```
For a truly immutable object, the class itself must guard its fields (e.g. make fields `private final` with no setters).

**2. `final` method** — can't be overridden by subclasses
```java
class Base { public final void config() { /*lock behavior*/ } }
class Sub extends Base { public void config(){} } // error
```
Use to freeze critical invariants (e.g. `Object.getClass()`, security-sensitive methods).

**3. `final` class** — can't be extended
```java
public final class String { ... }   // String, Integer, etc. are final
```
Enables safe caching (string pool) and thread-safety by construction.

## Effectively final (lambdas / inner classes)
A local variable used inside a lambda or anonymous class must be `final` or **effectively final** (never reassigned).
```java
int base = 10;                      // effectively final
Runnable r = () -> System.out.println(base + 1);   // OK
```

> [!tip] Immutability recipe
> `final class` + `private final` fields + no setters + defensive copies on input/output = an immutable value type. Immutable objects are inherently thread-safe ([[Multithreading]]) and make great `HashMap` keys.

> [!warning] Pitfalls
> - `final` on a reference does **not** freeze the object's fields — common confusion.
> - `final` fields must be set by the end of construction (in the constructor or initializer).
> - A `final` class breaks extension; ensure you don't need polymorphism there.

## Related
- [[Variables-and-Data-Types]] · [[Inheritance]] · [[Polymorphism]] · [[Multithreading]]
