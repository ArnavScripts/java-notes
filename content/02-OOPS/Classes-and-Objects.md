---
type: concept
tags:
  - java/oops
  - java/class
difficulty: easy
pattern: ""
related:
  - "[[Constructors]]"
  - "[[this-Keyword]]"
  - "[[static-Keyword]]"
  - "[[Object-Class]]"
  - "[[Encapsulation]]"
aliases:
  - Class
  - Object
---

# Classes & Objects

> [!note] Definition
> A **class** is a blueprint (fields + methods). An **object** is an instance living on the heap, referenced by a variable on the stack. `new` allocates and runs a constructor.

## Anatomy
```java
class Point {
    // fields (state)
    private int x, y;

    // constructor
    Point(int x, int y) { this.x = x; this.y = y; }

    // methods (behavior)
    public double distanceToOrigin() {
        return Math.sqrt(x*x + y*y);
    }
}
```
Create & use:
```java
Point p = new Point(3, 4);
System.out.println(p.distanceToOrigin());  // 5.0
```

## Memory model
- `Point p` — reference variable on the **stack**.
- `new Point(...)` — object on the **heap**; `p` holds its address.
- Passing `p` to a method copies the reference → method sees same object. See [[Methods]].

## Default values for fields
Objects are zeroed on creation: numbers `0`, `boolean false`, references `null`. **Local** variables have NO default — must be initialized before use.

> [!tip] Class vs instance
> Fields/methods without `static` belong to the **instance** (one copy per object). With `static` they belong to the **class** (one shared copy). See [[static-Keyword]].

> [!warning] Pitfalls
> - `Point q;` declares a reference but is `null` until `new`; calling `q.x` → `NullPointerException`.
> - Two references to the same object share state: `Point r = p; r.x = 99;` changes `p.x` too.
> - A `.java` file may have multiple classes but only **one** `public` (matching filename).

## Related
- [[Constructors]] · [[this-Keyword]] · [[static-Keyword]] · [[Object-Class]] · [[Encapsulation]]
