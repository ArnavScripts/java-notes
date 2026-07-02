---
type: concept
tags:
  - java/oops
  - design
  - solid
difficulty: medium
status: evergreen
related:
  - "[[Encapsulation]]"
  - "[[Abstraction-and-Interfaces]]"
  - "[[Polymorphism]]"
  - "[[Inheritance]]"
aliases:
  - SOLID
  - Design principles
---


# SOLID Principles

> [!note] Definition
> SOLID is five design heuristics for OOP that keep code maintainable, flexible, and testable. They're guidelines, not laws — apply where change/risk justifies the complexity.

## S — Single Responsibility
A class should have **one reason to change**. Cohesion high, mixing low.
```java
// bad: Report parses data, formats HTML, AND emails
class Report { /* 3 unrelated jobs -> 3 reasons to change */ }

// good: split
class ReportParser {}
class HtmlFormatter {}
class EmailSender {}
```

## O — Open/Closed
Open for **extension**, closed for **modification*. Add behavior via new subclasses/implementations, not by editing working code.
```java
interface Discount { double apply(double price); }
class SaleDiscount implements Discount { public double apply(double p){ return p*0.9; } }
// add a new discount without touching existing ones
```
Achieved via [[Polymorphism]] + [[Abstraction-and-Interfaces]].

## L — Liskov Substitution
Subtypes must be substitutable for their base types **without breaking behavior**. A subclass must honor the base's contract (preconditions not strengthened, postconditions not weakened, exceptions not widened).
> [!warning] Classic violation: `Square extends Rectangle` — `setW/h` semantics conflict. "Is-a" of structure ≠ is-a of behavior.

## I — Interface Segregation
Don't force clients to depend on methods they don't use. Many small, focused interfaces beat one fat one.
```java
// bad
interface Worker { void work(); void eat(); }
// good
interface Workable { void work(); }
interface Eatable  { void eat(); }
```

## D — Dependency Inversion
Depend on **abstractions**, not concretions. High-level modules shouldn't import low-level modules; both depend on interfaces.
```java
class Checkout {
    private final PaymentGateway gw;        // depend on interface
    Checkout(PaymentGateway gw){ this.gw = gw; }  // inject
}
```
This is dependency injection — enables mocking in tests and swapping implementations.

> [!tip] How they interlock
> SRP (cohesion) → OCP (extend via abstraction) → LSP (subtypes honor contracts) → ISP (small interfaces) → DIP (depend on abstractions). [[Polymorphism]] and [[Abstraction-and-Interfaces]] are the machinery that makes them work.

> [!tip] Pragmatism
> Don't pre-apply SOLID "just in case" — apply the part that addresses the **change axis you actually have**. Over-engineering violates the spirit of SRP at the system level.

## Related
- [[Encapsulation]] · [[Abstraction-and-Interfaces]] · [[Polymorphism]] · [[Inheritance]]
