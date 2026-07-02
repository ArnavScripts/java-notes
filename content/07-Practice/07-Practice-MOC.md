---
type: moc
tags:
  - moc
  - practice
status: evergreen
related:
  - "[[00-Index-MOC]]"
  - "[[06-Patterns-MOC]]"
  - "[[05-Algorithms-MOC]]"
  - "[[04-Data-Structures-MOC]]"
  - "[[Question-Template]]"
aliases:
  - Practice MOC
  - Question bank MOC
---


# Practice — MOC

The question bank. Every question carries `pattern:` and `difficulty:` metadata so Dataview can group them. Solve by **pattern**, not by difficulty alone — pattern recognition is the skill you're building.

## Buckets
- [[Easy-Questions]] — fundamentals, syntax, single-pattern warmups
- [[Medium-Questions]] — the core interview band; usually one clear pattern
- [[Hard-Questions]] — multi-pattern, edge cases, optimizations
- [[Pattern-Question-Map]] — every pattern → its representative questions

## How to use this bank
1. Learn a pattern in [[06-Patterns-MOC]].
2. Open [[Pattern-Question-Map]], find that pattern's questions.
3. Solve easy → medium → hard for that pattern.
4. For each, **first** identify the pattern and sketch the approach, **then** code.
5. Add new questions by copying [[Question-Template]] and setting `pattern:` + `difficulty:`.

> [!tip] The goal isn't "solve N problems" — it's "see the same pattern enough times that a new problem triggers recognition in seconds." Revisit a pattern's question set after a few days to confirm retention.

## Dataview: all questions by difficulty
```dataview
TABLE pattern, source, status
WHERE type = "question"
SORT difficulty ASC, file.name
```

## Dataview: questions grouped by pattern
```dataview
TABLE file.name as "Question", difficulty, source, status
WHERE type = "question"
GROUP BY pattern
SORT pattern
```

## Dataview: count per pattern
```dataview
TABLE length(rows) as "Count"
WHERE type = "question"
GROUP BY pattern
SORT pattern
```

## Dataview: seedling questions to fill
```dataview
TABLE pattern, difficulty
WHERE type = "question" AND status = "seedling"
SORT difficulty ASC, file.name
```
