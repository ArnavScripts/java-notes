---
type: concept
tags:
  - java/oops
  - java/inner
difficulty: medium
pattern: ""
related:
  - "[[static-Keyword]]"
  - "[[Classes-and-Objects]]"
  - "[[Abstraction-and-Interfaces]]"
aliases:
  - Nested classes
  - Anonymous class
---

# Inner Classes

> [!note] Definition
> A **nested class** is a class defined inside another. Four kinds: static nested, inner (non-static), local, and anonymous. The static-vs-non-static distinction decides whether it has a hidden reference to the enclosing instance.

## 1. Static nested class
```java
class Outer {
    static class Pair { int a, b; }   // no link to Outer instance
}
Outer.Pair p = new Outer.Pair();      // instantiate without an Outer
```
Behaves like a top-level class, just namespaced. **Preferred** unless you need the enclosing instance. (`LinkedList$Node` is usually modeled this way.)

## 2. Inner (non-static) class
```java
class Outer {
    int state;
    class Inner {
        void f() { System.out.println(state); } // can use Outer's state
    }
}
Outer o = new Outer();
Outer.Inner i = o.new Inner();    // needs an Outer instance
```
Holds a hidden `Outer.this` reference → can't exist without an enclosing instance; can't have `static` members.

## 3. Local class — declared inside a method
```java
void process() {
    class Helper { void run(){ /*...*/ } }
    new Helper().run();
}
```
Can capture effectively-final local vars.

## 4. Anonymous class — implement/use on the fly
```java
Runnable r = new Runnable() {
    public void run() { System.out.println("hi"); }
};
Comparator<String> byLen = new Comparator<>() {
    public int compare(String a, String b) { return a.length() - b.length(); }
};
```

> [!tip] Modern Java: prefer lambdas
> A functional interface (one abstract method) + anonymous class → replace with a **lambda** (`() -> ...`, `(a,b) -> ...`). Cleaner and less boilerplate. See [[Streams-and-Lambdas]]. Anonymous classes are still needed for multi-method interfaces or when you need state/`this` to mean the instance.

> [!warning] Pitfalls
> - Non-static inner classes carry the hidden outer reference → memory leak risk if the inner outlives the outer (e.g. inner registered as a listener).
> - Inside an anonymous class, `this` refers to the anonymous instance, **not** the enclosing class. Use `Outer.this` if needed.
> - Non-static inner class can't have `static` members (until Java 16 relaxed some of this).

## DSA usage
- A **static nested `Node`** class is the idiomatic way to define LL/tree nodes inside a solution.
```java
class Solution {
    static class Node { int val; Node next; Node(int v){val=v;} }
}
```

## Related
- [[static-Keyword]] · [[Classes-and-Objects]] · [[Abstraction-and-Interfaces]] · [[Streams-and-Lambdas]]
