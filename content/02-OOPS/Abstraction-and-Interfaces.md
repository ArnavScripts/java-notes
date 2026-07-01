---
type: concept
tags:
  - java/oops
  - java/abstraction
  - java/interfaces
difficulty: medium
pattern: ""
related:
  - "[[Inheritance]]"
  - "[[Polymorphism]]"
  - "[[SOLID-Principles]]"
aliases:
  - Abstract Class
  - Interface
  - default methods
---

# Abstraction & Interfaces

> [!note] Definition
> Abstraction exposes *what* an object does, hiding *how*. Java gives two tools: **abstract classes** (partial implementation, single inheritance) and **interfaces** (pure contract, multiple implementation).

## Abstract class
```java
abstract class Shape {
    abstract double area();          // no body; subclasses must implement
    void describe() {               // concrete method allowed
        System.out.println("area = " + area());
    }
}
class Circle extends Shape {
    double r;
    Circle(double r) { this.r = r; }
    @Override double area() { return Math.PI * r * r; }
}
```
- Can have fields, constructors, concrete + abstract methods.
- Can't be instantiated: `new Shape()` is illegal.
- A subclass must implement all abstract methods or itself be `abstract`.

## Interface
```java
interface Comparable<T> {
    int compareTo(T other);          // implicitly public abstract
}

interface Drawable {
    default void info() { System.out.println("drawable"); } // Java 8+
    static Drawable empty() { return () -> {}; }             // Java 8+ static
}
class Point implements Comparable<Point>, Drawable {
    int x, y;
    public int compareTo(Point o) { return Integer.compare(x, o.x); }
}
```
- All fields are implicitly `public static final` (constants).
- Methods are `public abstract` unless `default`/`static`/`private` (Java 9+).
- A class can `implements` **multiple** interfaces (Java's answer to multiple inheritance of type).

## Abstract class vs interface
| Aspect | Abstract class | Interface |
|--------|----------------|-----------|
| Inheritance | single `extends` | multiple `implements` |
| Fields | any | `public static final` only |
| Constructors | yes | no |
| State | yes | no (constants only) |
| Use when | share code + state | define a capability/contract |

> [!tip] Design heuristic
> Prefer **interfaces** for capabilities (`Comparable`, `Iterable`, `Drawable`). Use **abstract classes** only when subclasses share meaningful code *and* state along a true IS-A line. See [[SOLID-Principles]] (Dependency Inversion, Interface Segregation).

## `default` methods & the diamond
```java
interface A { default void f() { System.out.println("A"); } }
interface B { default void f() { System.out.println("B"); } }
class C implements A, B {
    public void f() { A.super.f(); }   // MUST override to resolve conflict
}
```

> [!warning] Pitfalls
> - Forgetting `public` on an interface method implementation → compile error (interface methods are `public`).
> - An interface with `default` methods that mutate state via fields is impossible — interfaces have no instance state.
> - Over-relying on `default` methods turns interfaces into multiple inheritance of behavior → keep them minimal.

## Related
- [[Inheritance]] · [[Polymorphism]] · [[SOLID-Principles]] · [[Generics]] (`Comparable<T>`)
