---
type: concept
tags:
  - java/advanced
  - java/collections
difficulty: medium
pattern: ""
related:
  - "[[Generics]]"
  - "[[Arrays]]"
  - "[[HashMap]]"
  - "[[HashSet]]"
  - "[[Linked-List]]"
  - "[[Stack]]"
  - "[[Queue]]"
  - "[[Heap]]"
  - "[[Object-Class]]"
aliases:
  - Collections Framework
  - Java Collections
---

# Collections Framework

> [!note] Definition
> A unified architecture in `java.util` for storing/processing groups of objects: `List` (ordered, indexed), `Set` (no duplicates), `Queue`/`Deque` (FIFO/ends), `Map` (key→value). All generic, all iterable.

## Hierarchy at a glance
```
Collection (interface)
├── List      -> ArrayList, LinkedList, Vector/Stack
├── Set       -> HashSet, LinkedHashSet, TreeSet
└── Queue/Deque -> ArrayDeque, PriorityQueue, LinkedList
Map (interface, NOT a Collection)
└── HashMap, LinkedHashMap, TreeMap, Hashtable
```

## Choosing the right collection
| Need | Use |
|------|-----|
| Random access list | `ArrayList` |
| Frequent head/tail ops | `ArrayDeque` (faster than `LinkedList`) |
| Unique items, fast contains | `HashSet` (O(1)) |
| Unique + sorted | `TreeSet` (O(log n)) |
| Unique + insertion order | `LinkedHashSet` |
| Key→value, fast lookup | `HashMap` (O(1)) |
| Sorted keys | `TreeMap` (O(log n)) |
| Insertion-order keys | `LinkedHashMap` |
| Min/max priority | `PriorityQueue` ([[Heap]]) |

## Core operations (List example)
```java
List<Integer> list = new ArrayList<>();
list.add(1); list.add(2); list.add(3);
list.get(0);            // 1
list.set(1, 99);        // [1,99,3]
list.remove(0);         // [99,3]
list.size(); list.contains(99); list.isEmpty();
Collections.sort(list); Collections.reverse(list);
Collections.max(list); Collections.frequency(list, 99);
```

## Iteration styles
```java
for (int x : list) {}                       // enhanced-for
list.forEach(x -> System.out.println(x));   // Iterable.forEach
Iterator<Integer> it = list.iterator();
while (it.hasNext()) it.next(); it.remove(); // safe removal during iteration
```

## Map essentials
```java
Map<String,Integer> m = new HashMap<>();
m.put("a",1); m.get("a"); m.getOrDefault("b",0);
m.containsKey("a"); m.keySet(); m.values();
m.forEach((k,v) -> {});
for (var e : m.entrySet()) { e.getKey(); e.getValue(); }
```

> [!tip] DSA defaults
> - `ArrayList<Integer>` when you need a growable array.
> - `HashMap` for counting/frequency maps (most-used DSA structure). Use `getOrDefault` to avoid null checks.
> - `ArrayDeque` as a fast stack **and** queue (replaces legacy `Stack` and `LinkedList`-as-queue).
> - `PriorityQueue` for [[Heap]] / top-k problems.
> - `TreeMap`/`TreeSet` when you need **ordered** keys / ceiling/floor queries.

> [!warning] Pitfalls
> - `Stack` is synchronized and legacy — prefer `ArrayDeque`.
> - `LinkedList` implements `List` *and* `Deque`; as a `List` it's O(n) random access.
> - Modifying a collection while iterating with enhanced-for → `ConcurrentModificationException`. Use `Iterator.remove()` or `removeIf`.
> - Using a **mutable object as a key** and then mutating it → it disappears from the map (hash changes). Keep keys immutable ([[final-Keyword]], [[Object-Class]]).
> - Raw types (`List` without `<T>`) lose type safety — always parameterize ([[Generics]]).

## Related
- [[Generics]] · [[Arrays]] · [[HashMap]] · [[HashSet]] · [[Linked-List]] · [[Stack]] · [[Queue]] · [[Heap]] · [[Object-Class]]
