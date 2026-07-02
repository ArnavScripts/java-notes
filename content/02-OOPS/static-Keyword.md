---
type: concept
tags:
  - java/oops
  - java/static
difficulty: easy
status: evergreen
related:
  - "[[Classes-and-Objects]]"
  - "[[Methods]]"
  - "[[Inheritance]]"
aliases:
  - static
  - Class variables
---


# `static` Keyword

> [!note] Definition
> `static` means "belongs to the **class**, not an instance". One shared copy regardless of how many objects exist. Accessed via `ClassName.member`.

## Four uses

**1. Static fields** — shared state / constants
```java
class Counter {
    static int count = 0;          // one counter for all instances
    static final double PI = 3.14; // compile-time constant
    Counter() { count++; }
}
Counter.count;   // class-level access
```

**2. Static methods** — no `this`, can't directly use instance fields
```java
static int max(int a, int b) { return a > b ? a : b; }
Math.max(2, 3);   // canonical example
```

**3. Static blocks** — one-time class initialization
```java
static {
    // runs once when class is loaded, before main()
    loadConfig();
}
```

**4. Static nested classes** — see [[Inner-Classes]]

## `static final` constants
```java
public static final int MAX = 100;   // naming: UPPER_SNAKE
```
Inlined by the compiler when possible → no runtime lookup.

> [!tip] DSA usage
> `static` methods are the contest `main`'s helpers: `static void solve(BufferedReader r){...}`. Common in LeetCode-style because `Solution` has only instance methods, but in raw CP code everything is `static`.

## Memory layout: instance vs static
```
Heap
├── Object A  ────────────────┐
│   └── instance field x = 10 │
├── Object B                  ├──>  Class area
│   └── instance field x = 20 │     └── static count = 2
└─────────────────────────────┘
```

## Static imports (readability shortcut)
```java
import static java.lang.Math.*;
double r = sqrt(PI);   // instead of Math.sqrt(Math.PI)
```
Use sparingly — overuse hurts readability.

## Static in a nutshell
| Use | Declared | Accessed | Has `this`? |
|-----|----------|----------|-------------|
| Field | `static int count` | `Class.count` | No |
| Method | `static int max(...)` | `Class.max(...)` | No |
| Block | `static { ... }` | On class load | No |
| Nested class | `static class Node` | `Outer.Node` | No |

> [!warning] Pitfalls
> - A static method **cannot** be overridden (no dynamic dispatch) — it's "hidden" if redefined in a subclass. `@Override` will refuse it.
> - Static state is shared across all instances and threads — beware in [[Multithreading]].
> - Overusing `static` breaks OOP (procedural code in a class skin) and hurts testability.

## Related
- [[Classes-and-Objects]] · [[Methods]] · [[Inheritance]] · [[Inner-Classes]]
