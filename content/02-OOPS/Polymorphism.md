---
type: concept
tags:
  - java/oops
  - java/polymorphism
difficulty: medium
status: evergreen
related:
  - "[[Inheritance]]"
  - "[[Abstraction-and-Interfaces]]"
  - "[[Object-Class]]"
  - "[[Methods]]"
aliases:
  - Overloading
  - Overriding
  - Dynamic dispatch
---


# Polymorphism

> [!note] Definition
> Polymorphism = "many forms" — one reference type can hold many concrete types, and the right method is chosen at runtime. Two faces in Java: **compile-time** (overloading) and **runtime** (overriding + dynamic dispatch).

## Overloading (compile-time / static)
Same method name, different parameter list, **same** class. Resolved by the compiler from argument types.
```java
int  add(int a, int b)       { return a + b; }
double add(double a, double b){ return a + b; }
```
See [[Methods]].

## Overriding (runtime / dynamic)
A subclass redefines an inherited method with the **same signature**. Resolved at runtime based on the actual object type.
```java
class Animal { void sound() { System.out.println("..."); } }
class Cat extends Animal { @Override void sound() { System.out.println("meow"); } }

Animal a = new Cat();
a.sound();   // "meow"  <- dynamic dispatch picks Cat's version
```

## Rules for overriding
- Same name + same params + compatible (same/covariant) return type.
- Access can't be more restrictive (parent `protected` → child `protected`/`public`).
- Can't throw new/wider **checked** exceptions.
- `private`, `static`, `final` methods are **not** overridden (`static` is *hidden* — see [[static-Keyword]]).
- Always annotate `@Override` so the compiler checks you really overrode.

## Upcasting & the polymorphic payoff
```java
Animal[] zoo = { new Cat(), new Dog(), new Animal() };
for (Animal z : zoo) z.sound();   // each makes its own sound
```
Code written against the **supertype** works for any subtype → open/closed. See [[SOLID-Principles]].

> [!tip] Pattern recognition
> "Call a method on a base/iface reference, want the subclass behavior" → that's runtime polymorphism. It's how interfaces ([[Abstraction-and-Interfaces]]) become useful.

> [!warning] Pitfalls
> - `Animal a = new Cat(); a.someCatOnlyMethod();` — won't compile; reference type limits visible API.
> - Field access is **not** polymorphic: `a.field` uses the *reference* type's field.
> - Calling an overridable method from a constructor leaks the half-built `this` to the subclass — avoid.

## Related
- [[Inheritance]] · [[Abstraction-and-Interfaces]] · [[Object-Class]] · [[Methods]] · [[SOLID-Principles]]
