---
type: concept
tags:
  - java/advanced
  - java/exceptions
difficulty: medium
pattern: ""
related:
  - "[[Exception-Basics]]"
  - "[[Abstraction-and-Interfaces]]"
  - "[[SOLID-Principles]]"
aliases:
  - Exception Handling
  - Custom exceptions
---

# Exception Handling (Design Level)

> [!note] Definition
> Building on [[Exception-Basics]], this is the *design* view: when to throw, what type, how to layer handling across a call stack, and how to add your own exception types.

## The hierarchy (recap)
```
Throwable
├── Error                 // JVM-level (OOM, StackOverflow) — don't catch
└── Exception
    └── RuntimeException  // unchecked (NPE, IndexOOB, Arithmetic)
```
- **Checked** = forces API client to acknowledge (`IOException`, `SQLException`).
- **Unchecked** = programmer errors / runtime invariants (`IllegalArgumentException`).

## Custom exceptions
```java
class InvalidAgeException extends IllegalArgumentException {
    InvalidAgeException(String msg){ super(msg); }     // unchecked lineage
}
class DataLoadException extends Exception {            // checked
    DataLoadException(String msg, Throwable cause){ super(msg, cause); }
}
```
Choose the lineage by intent: programming bug → `RuntimeException` subclass; recoverable external condition → checked `Exception` subclass.

## Throw early, catch late
- **Throw early**: validate at boundaries (`if (n < 0) throw new IllegalArgumentException(...)`), fail fast with a precise message.
- **Catch late**: handle where you can actually *do* something (retry, log, show user). Don't catch just to swallow.
- **Wrap & rethrow** to translate low-level errors into domain ones, preserving the cause:
```java
try { loadFile(); }
catch (IOException e) { throw new DataLoadException("config unreadable", e); }
```
The `cause` keeps the original stack trace.

## try-with-resources (Java 7+) — auto-close
```java
try (BufferedReader br = new BufferedReader(new FileReader(p))) {
    String line; while ((line = br.readLine()) != null) { /*...*/ }
}   // br.close() called automatically even on exception
```
Resource must implement `AutoCloseable`. Multiple resources allowed; closed in reverse order.

> [!tip] When checked exceptions hurt
> Checked exceptions propagate through every intermediate method's signature, breaking encapsulation and lambdas (lambdas can't throw checked). Many modern APIs favor unchecked exceptions + a single top-level handler. Use your judgment per codebase.

> [!warning] Pitfalls
> - Catching `Exception`/`Throwable` broadly hides bugs and `Error`s — catch the specific type.
> - Empty `catch {}` (swallow) is the #1 debugging nightmare.
> - Don't use exceptions for normal control flow (they're ~1000× slower than an `if`).
> - A `finally` that `return`s silently overrides a thrown exception — never do this.

## Related
- [[Exception-Basics]] · [[Abstraction-and-Interfaces]] · [[IO-and-NIO]] (try-with-resources) · [[SOLID-Principles]]
