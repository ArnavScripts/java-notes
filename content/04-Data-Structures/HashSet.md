---
type: concept
tags:
  - dsa/ds
  - java/ds/hashset
difficulty: easy
status: evergreen
related:
  - "[[HashMap]]"
  - "[[Object-Class]]"
  - "[[Two-Pointers]]"
  - "[[Cyclic-Sort]]"
  - "[[Strings-DS]]"
aliases:
  - HashSet
  - Set DS
---


# HashSet

> [!note] Definition
> A `HashSet<E>` stores **unique** elements using a `HashMap` under the hood (element → dummy value). Average O(1) `add`/`remove`/`contains`. Models membership, deduplication, and seen-tracking.

## Core API
```java
Set<Integer> set = new HashSet<>();
set.add(1); set.add(1);     // second add ignored
set.contains(1);            // true
set.remove(1);
set.size(); set.isEmpty();
for (int x : set) {}
set.forEach(x -> {});
```

## Variants
| Set | Ordering | Complexity |
|-----|----------|-----------|
| `HashSet` | none | O(1) avg |
| `LinkedHashSet` | insertion order | O(1) avg |
| `TreeSet` | sorted (natural/comparator) | O(log n) |

`TreeSet` adds: `first()`, `last()`, `ceiling(x)`, `floor(x)`, `higher(x)`, `lower(x)`, `headSet/tailSet/subSet`.

## Idioms
**Deduplicate:**
```java
List<Integer> uniq = new ArrayList<>(new LinkedHashSet<>(list));  // keeps order
```
**Seen-tracking (BFS/DFS avoid revisits):**
```java
Set<Node> seen = new HashSet<>(); seen.add(start);
```
**Set operations:**
```java
set1.retainAll(set2);   // intersection (in place)
set1.removeAll(set2);   // difference
set1.addAll(set2);      // union
```

> [!tip] Pattern recognition cues
> - "Check if an element was seen" → `HashSet` (`contains` in O(1)).
> - "Find the duplicate / first repeating" → add and detect, or boolean array for small ranges.
> - "Array intersection / union" → sets.
> - "Numbers 1..n, find missing/duplicate" → [[Cyclic-Sort]] or a set.
> - "Need ceiling/floor/sorted order" → `TreeSet` (or sort + binary search).

## Worked: contains duplicate
```java
boolean hasDup(int[] a) {
    Set<Integer> s = new HashSet<>();
    for (int x : a) if (!s.add(x)) return true;   // add returns false if present
    return false;
}
```

## Worked: TreeSet for "closest greater"
```java
TreeSet<Integer> ts = new TreeSet<>();
ts.add(10); ts.add(20); ts.add(30);
ts.ceiling(15);   // 20  (smallest >= 15)
ts.floor(15);     // 10  (largest  <= 15)
ts.higher(20);    // 30  (strictly greater)
```

> [!warning] Pitfalls
> - Custom element type → must override `equals`/`hashCode` ([[Object-Class]]), else duplicates "appear".
  - `TreeSet` requires `Comparable` or a `Comparator`; uses `compareTo` (not `equals`) — two "equal via compareTo" elements collapse.
  - For small bounded ranges (1..n, ASCII), a `boolean[]`/`int[128]` is far faster than a set.
  - Iteration order of `HashSet` is not guaranteed and can change across runs.

## Practice questions
- [[Contains-Duplicate]] → HashSet
- [[Intersection-of-Two-Arrays]] → set retainAll
- [[First-Missing-Positive]] → [[Cyclic-Sort]] / index marking
- [[Longest-Consecutive-Sequence]] → HashSet + streak expansion

## Related
- [[HashMap]] · [[Object-Class]] · [[Two-Pointers]] · [[Cyclic-Sort]] · [[Strings-DS]]
