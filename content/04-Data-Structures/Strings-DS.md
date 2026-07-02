---
type: concept
tags:
  - dsa/ds
  - java/ds/string
difficulty: easy
status: evergreen
related:
  - "[[Strings]]"
  - "[[Arrays]]"
  - "[[Two-Pointers]]"
  - "[[Sliding-Window]]"
  - "[[HashMap]]"
  - "[[Trie]]"
aliases:
  - String DS
  - Substring
---


# Strings (DS view)

> [!note] Definition
> A string is a sequence of characters. In Java, `String` is immutable (backed by `char[]`/byte[]); `StringBuilder`/`char[]` are the mutable workhorses for DSA. Treat a string as an array of chars for most algorithmic thinking.

## Why a separate DS note from [[Strings]]?
The DS view is about **operations and patterns** (substring, anagram, palindrome, matching), whereas the syntax note covers the API. Most string algorithms reduce to array techniques with char-index tricks.

## Key representations
```java
String s = "abcba";
char[] c = s.toCharArray();          // mutable, O(n) copy
int n = s.length();
int idx = s.charAt(i) - 'a';         // 0..25 for lowercase (array-of-26 trick)
```

## Frequency / count array (no map needed for limited alphabets)
```java
int[] freq = new int[26];
for (char ch : s.toCharArray()) freq[ch - 'a']++;
```
> [!tip] For lowercase/uppercase/digits only, an `int[26]`/`int[128]` is faster and lighter than a `HashMap`. Use a `HashMap` when the alphabet is large/Unicode.

## Common patterns
- **Palindrome**: two pointers inward (`[[Two-Pointers]]`), or expand-around-center (longest palindromic substring).
- **Anagram / permutation check**: equal frequency arrays (sort or count).
- **Substring matching**: naive O(nm), KMP O(n+m), Rabin-Karp rolling hash ([[Sliding-Window]]-flavored).
- **Longest substring with constraint**: [[Sliding-Window]] (k distinct, no repeats, longest vowel substring).
- **Prefix problems**: [[Trie]] or `String.startsWith`.
- **Rotations / equality**: double the string (`s+s`) and search.

## Rolling hash (Rabin-Karp flavor)
```java
long h = 0; long P = 31, M = 1_000_000_007L;
for (int i = 0; i < m; i++) h = (h*P + s.charAt(i)) % M;
// slide: remove left char, add right char, using powers of P
```
Used for substring search and "find repeated substring of length L" (binary search on L + hash).

> [!tip] Pattern recognition cues
> - "Longest substring ..." → [[Sliding-Window]].
> - "Are these permutations/anagrams" → frequency arrays.
> - "All prefixes of many words" / "autocomplete" → [[Trie]].
> - "Match pattern in text, repeated queries" → KMP / Rabin-Karp.
> - "Palindrome" → [[Two-Pointers]] or expand-center.
> - "Build/transform string with prepends" → `StringBuilder` (avoid `+=`).

> [!warning] Pitfalls
> - `s.charAt(i)` is O(1) but repeated in hot loops is slower than `toCharArray()` once and indexing the array.
> - Building with `+=` → O(n²); use `StringBuilder`.
> - Char arithmetic: `'A'` vs `'a'`; uppercase is 65, lowercase 97 — don't mix.
> - Hash collisions in Rabin-Karp → double-check equality on hash match.

## Practice questions
- [[Valid-Palindrome]] → [[Two-Pointers]]
- [[Valid-Anagram]] → frequency arrays
- [[Longest-Substring-Without-Repeating-Characters]] → [[Sliding-Window]]
- [[Group-Anagrams]] → [[HashMap]] + sorted-key
- [[Longest-Palindromic-Substring]] → expand center / DP

## Related
- [[Strings]] (API) · [[Arrays]] · [[Two-Pointers]] · [[Sliding-Window]] · [[HashMap]] · [[Trie]] · [[DP]]
