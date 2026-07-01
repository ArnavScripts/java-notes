---
type: concept
tags:
  - java/ds/hashmap
difficulty: medium
pattern: ""
related:
  - "[[HashSet]]"
  - "[[Object-Class]]"
  - "[[Two-Pointers]]"
  - "[[Sliding-Window]]"
  - "[[Prefix-Sum]]"
  - "[[Strings-DS]]"
aliases:
  - HashMap
  - Hash table
  - Frequency map
---

# HashMap

> [!note] Definition
> A `HashMap<K,V>` stores key→value pairs in buckets keyed by `hashCode()`. Average O(1) `get`/`put`/`remove`; worst-case O(log n) since Java 8 (treeifies buckets at ≥8 entries). Not ordered, not thread-safe.

## Core API
```java
Map<String,Integer> m = new HashMap<>();
m.put("a", 1);
m.get("a");               // 1 or null
m.getOrDefault("b", 0);   // 0  <-- avoids null check, DSA favorite
m.containsKey("a"); m.containsValue(1);
m.remove("a");
m.putIfAbsent("c", 9);
m.merge("a", 1, Integer::sum);   // add 1 to current (great for counters)
m.forEach((k,v) -> {});
for (var e : m.entrySet()) { e.getKey(); e.getValue(); }
```

## Frequency / counter idiom (DSA gold)
```java
Map<Integer,Integer> freq = new HashMap<>();
for (int x : arr) freq.merge(x, 1, Integer::sum);   // count occurrences
// or the classic:
for (int x : arr) freq.put(x, freq.getOrDefault(x, 0) + 1);
```

## How it works
- `hash = (h = key.hashCode()) ^ (h >>> 16)` → spreads high bits.
- `index = (n-1) & hash` → bucket (n is power-of-2 capacity).
- Bucket: linked list; treeified to red-black tree when size ≥8 & table ≥64.
- Rehash on load factor threshold (default 0.75).

## Why `equals`/`hashCode` matter
A custom key **must** override both consistently — see [[Object-Class]]. Violate the contract and `get` returns `null` for a key that's "in" the map.

> [!tip] Pattern recognition cues (HashMap is everywhere)
> - "Two-sum / pair-sum / count pairs with property" → value→index or value→count map ([[Two-Pointers]] alternative).
> - "Longest substring with k distinct / exactly k" → frequency map + [[Sliding-Window]].
> - "Subarray sum equals k" → prefix-sum → count map ([[Prefix-Sum]]).
> - "Group items by a key" → `Map<K,List<V>>` with `computeIfAbsent`.
> - "Anagrams" → sorted string key → list of originals ([[Strings-DS]]).
> - "Memoization in DP" → `Map<state, result>` ([[DP]]).

## Worked: two-sum (one pass)
```java
int[] twoSum(int[] a, int target) {
    Map<Integer,Integer> seen = new HashMap<>();      // value -> index
    for (int i = 0; i < a.length; i++) {
        Integer j = seen.get(target - a[i]);
        if (j != null) return new int[]{j, i};
        seen.put(a[i], i);
    }
    return new int[]{-1,-1};
}
```

## Worked: group by key
```java
Map<String,List<String>> groups = new HashMap<>();
for (String w : words)
    groups.computeIfAbsent(sortKey(w), k -> new ArrayList<>()).add(w);
```

> [!warning] Pitfalls
> - Custom key without `equals`/`hashCode` → silent "missing key" bugs.
> - Mutating a key after insertion → it disappears (hash changes). Keep keys immutable.
> - `null` is allowed as a key (once) and as values in `HashMap`; **not** in `Hashtable`/`ConcurrentHashMap`.
> - Iteration order is unspecified → use `LinkedHashMap` (insertion) or `TreeMap` (sorted) when you need order.
> - Capacity/load factor: pre-size with `new HashMap<>(expectedSize)` for known sizes to avoid rehashing.
> - Worst case O(n) before treeification; for adversarial inputs Java 8+ keeps it O(log n).

## Practice questions
- [[Two-Sum]] → hash map
- [[Subarray-Sum-Equals-K]] → [[Prefix-Sum]] + map
- [[Group-Anagrams]] → grouped map
- [[Longest-Substring-Without-Repeating-Characters]] → map + window
- [[Top-K-Frequent-Elements]] → freq map + [[Heap]]

## Related
- [[HashSet]] · [[Object-Class]] · [[Two-Pointers]] · [[Sliding-Window]] · [[Prefix-Sum]] · [[Strings-DS]] · [[DP]]
