---
type: moc
tags:
  - moc
  - java/advanced
status: evergreen
related:
  - "[[00-Index-MOC]]"
  - "[[02-OOPS-MOC]]"
  - "[[04-Data-Structures-MOC]]"
aliases:
  - Java Advanced MOC
---


# Java Advanced — MOC

The parts of Java that turn "I can write a loop" into "I can write efficient, idiomatic Java for DSA and real systems."

## Notes
- [[Collections-Framework]] — List/Set/Queue/Map hierarchy, choosing the right one
- [[Generics]] — type parameters, bounds, type erasure
- [[Exception-Handling]] — design-level error handling, custom exceptions
- [[Streams-and-Lambdas]] — functional-style pipelines, `Optional`, method refs
- [[IO-and-NIO]] — files, streams of bytes/chars, `Path`, buffered I/O
- [[Multithreading]] — threads, synchronization, `ExecutorService`, concurrency
- [[JVM-and-Memory]] — stack/heap, garbage collection, class loading

> [!tip] Prerequisite
> Comfortable with [[02-OOPS-MOC|OOPS]] (interfaces, generics lean on it) and [[04-Data-Structures-MOC|DS]] (collections implement them).

## Dataview: this section
```dataview
TABLE difficulty, status, related as "Links"
WHERE file.folder = "03-Java-Advanced" AND type = "concept"
SORT file.name
```

## Dataview: thin notes to expand
```dataview
TABLE file.name as "Note", length(file.content) as "Chars"
WHERE file.folder = "03-Java-Advanced" AND type = "concept" AND length(file.content) < 3000
SORT length(file.content) ASC
```
