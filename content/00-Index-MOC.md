---
type: moc
tags:
  - moc
  - java
status: evergreen
related:
  - "[[01-Java-Syntax-MOC]]"
  - "[[02-OOPS-MOC]]"
  - "[[03-Java-Advanced-MOC]]"
  - "[[04-Data-Structures-MOC]]"
  - "[[05-Algorithms-MOC]]"
  - "[[06-Patterns-MOC]]"
  - "[[07-Practice-MOC]]"
aliases:
  - Home
  - Index
---


# Java / OOPS / DS / DSA — Master Index

A practical, interlinked knowledge web. Start anywhere; every note links back to its MOC and out to related concepts, forming a bidirectional graph.

## Learning path
1. **[[01-Java-Syntax-MOC]]** — language fundamentals: types, control flow, methods, strings, I/O
2. **[[02-OOPS-MOC]]** — classes, inheritance, polymorphism, SOLID
3. **[[03-Java-Advanced-MOC]]** — collections, generics, streams, threads, JVM
4. **[[04-Data-Structures-MOC]]** — arrays → graphs → tries
5. **[[05-Algorithms-MOC]]** — complexity, sorting, recursion, DP, greedy
6. **[[06-Patterns-MOC]]** — *the pattern-recognition core*: two pointers, sliding window, etc.
7. **[[07-Practice-MOC]]** — question bank mapped by pattern + difficulty

> [!tip] How to use this vault
> - Learn a concept → solve its linked questions → tag new questions with `pattern:` so Dataview auto-indexes them.
> - When stuck on a problem, ask: *"Which [[06-Patterns-MOC|pattern]] does this smell like?"*

## Dataview: all concept notes
```dataview
TABLE difficulty, status, pattern, file.folder as "Section"
WHERE type = "concept"
SORT file.folder, file.name
```

## Dataview: all questions by difficulty
```dataview
TABLE pattern, source, file.folder as "Bucket"
WHERE type = "question"
SORT difficulty ASC, file.name
```

## Dataview: questions grouped by pattern
```dataview
TABLE file.name as "Question", difficulty, source
WHERE type = "question"
GROUP BY pattern
SORT pattern
```

## Dataview: thin notes to expand
```dataview
TABLE file.folder as "Section", length(file.content) as "Chars"
WHERE type = "concept" AND length(file.content) < 3000
SORT length(file.content) ASC
```

## Templates
- [[Note-Template]] — for concepts
- [[Question-Template]] — for practice problems
- [[MOC-Template]] — for new sections

## Canvas maps
- [[Java-Learning-Roadmap.canvas]]
- [[OOPS-Pillars.canvas]]
- [[DSA-Pattern-Map.canvas]]
- [[JVM-Memory-Model.canvas]]
