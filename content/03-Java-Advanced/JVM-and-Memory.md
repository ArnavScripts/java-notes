---
type: concept
tags:
  - java/advanced
  - java/jvm
difficulty: hard
pattern: ""
related:
  - "[[Intro-and-Setup]]"
  - "[[Multithreading]]"
  - "[[Object-Class]]"
  - "[[Strings]]"
aliases:
  - JVM
  - Garbage collection
  - Memory model
---

# JVM & Memory

> [!note] Definition
> The JVM runs bytecode in runtime data areas: a per-thread **stack** for frames, a shared **heap** for objects, a **metaspace** for class metadata, plus the method area and PC/register. **Garbage collection** reclaims unreachable heap objects.

## Runtime data areas
| Area | Holds | Thread-shared? |
|------|-------|----------------|
| Stack | frames (locals, operands, calls) | per-thread |
| Heap | all objects + arrays | shared |
| Metaspace | class metadata (replaces PermGen) | shared |
| PC register | current instruction pointer | per-thread |
| Native stack | native calls | per-thread |

## Stack frames & local vars
Each method call pushes a frame: local variables (primitives inline, references to heap), operand stack, return value. Deep recursion → `StackOverflowError` (frame per call). Tune with `java -Xss` (smaller stack → more threads, less recursion depth).

## Heap & garbage collection
- Generational: **young** (Eden + 2 Survivor) → **old** → objects that survive move up.
- GC roots: local vars, active threads, static fields, JNI refs.
- An object is reachable if a path from a root exists; else eligible for collection.
- Common collectors: G1 (default since Java 9), ZGC, Shenandoah (low-pause).

## Memory leaks in Java (yes, they exist)
- Lingering references: static collections, caches, listeners, unclosed resources.
- `finalize()`/`Cleaner` delaying reclamation.
- Detune: profile with `jmap`/`jconsole`/VisualVM; look for growing retained sets.

## Class loading & "once" semantics
- A class is loaded **once** per classloader → `static` initializers run once. That's why `static final` constants and singletons work. See [[static-Keyword]].
- `ClassLoader` hierarchy: bootstrap → platform → app. Can be customized (hot reload, isolation).

## Tuning flags (context for interviews)
```
-Xms2g -Xmx4g      min/max heap
-Xss512k           per-thread stack
-XX:+UseG1GC       select collector
```

> [!tip] DSA-relevant facts
> - **Stack overflow** from deep recursion (e.g. naive recursion on a 10^5 list) → switch to iterative or raise `-Xss`. See [[Recursion]].
> - **Object creation cost**: each `new`/boxing allocates on heap → GC pressure. Prefer primitive arrays and reuse `StringBuilder`/`int[]` over autoboxing in hot loops.
> - `String` pool + intern: compare with `==` only for interned literals; otherwise [[Object-Class|equals]].

> [!warning] Pitfalls
> - `int[]` lives on heap too (arrays are objects); the reference is on the stack.
> - Static state lives as long as the class is loaded (whole app lifetime) — leaks.
> - Catching `OutOfMemoryError` is usually futile; fix the leak or raise the heap.

## Related
- [[Intro-and-Setup]] · [[Multithreading]] (JMM) · [[Object-Class]] · [[Strings]] · [[static-Keyword]]
