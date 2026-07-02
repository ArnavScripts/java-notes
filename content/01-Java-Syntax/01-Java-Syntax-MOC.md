---
type: moc
tags:
  - moc
  - java/syntax
status: evergreen
related:
  - "[[00-Index-MOC]]"
  - "[[02-OOPS-MOC]]"
aliases:
  - Syntax MOC
---


# Java Syntax — MOC

The language foundations. Master these before OOPS and DSA in Java.

## Notes
- [[Intro-and-Setup]] — JDK/JRE/JVM, hello world, compilation
- [[Variables-and-Data-Types]] — primitives, wrappers, casting
- [[Operators]] — arithmetic, relational, logical, bitwise
- [[Control-Flow]] — if/else, switch, ternary
- [[Loops]] — for, while, do-while, enhanced-for, break/continue
- [[Arrays-Basics]] — declaration, traversal, 2D arrays
- [[Methods]] — params, return, overloading, pass-by-value
- [[Strings]] — immutability, StringBuilder, common methods
- [[Input-Output]] — Scanner, PrintWriter, BufferedReader
- [[Exception-Basics]] — try/catch, checked vs unchecked

> [!tip] Prerequisite chain
> Intro → Types → Operators → Control Flow → Loops → Arrays → Methods → Strings → I/O → Exceptions → [[02-OOPS-MOC|OOPS]]

## Dataview: this section
```dataview
TABLE difficulty, status, related as "Links"
WHERE file.folder = "01-Java-Syntax" AND type = "concept"
SORT file.name
```

## Dataview: thin notes to expand
```dataview
TABLE file.name as "Note", length(file.content) as "Chars"
WHERE file.folder = "01-Java-Syntax" AND type = "concept" AND length(file.content) < 3000
SORT length(file.content) ASC
```
