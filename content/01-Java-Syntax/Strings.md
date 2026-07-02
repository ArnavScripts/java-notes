---
type: concept
tags:
  - java/syntax
  - java/strings
difficulty: easy
status: evergreen
related:
  - "[[Variables-and-Data-Types]]"
  - "[[Strings-DS]]"
  - "[[Input-Output]]"
aliases:
  - Java String
  - StringBuilder
---


# Strings

> [!note] Definition
> `String` is an **immutable** sequence of UTF-16 chars backed by a `char[]` (Java 8) or byte array (Java 9+, compact strings). `StringBuilder` is the mutable, non-synchronized companion for building strings efficiently.

## Immutability
```java
String s = "abc";
s.concat("def");     // returns NEW string "abcdef"; s is still "abc"
s = s.concat("def"); // reassign to keep the result
```
String pool: `String a = "hi"; String b = "hi";` → `a == b` is `true` (pool). `new String("hi")` bypasses the pool → `==` false.

## Common methods
```java
s.length()           // int (method, unlike array.length field)
s.charAt(i)          // char
s.substring(i)       // i..end
s.substring(i, j)    // i..j-1
s.indexOf("xy")      // first index or -1
s.equals(t)          // content compare (use this, not ==)
s.compareTo(t)       // <0, 0, >0  (lexicographic)
s.toCharArray()      // char[]
String.valueOf(arr)  // char[] -> String
s.split(",")         // String[]
s.trim() / s.strip()
s.replace('a','b')
s.toLowerCase()
```

## StringBuilder (mutable, efficient)
```java
StringBuilder sb = new StringBuilder();
sb.append("a").append(2).append(true);   // "a2true"
sb.reverse();
sb.insert(0, "x");
sb.deleteCharAt(sb.length()-1);
String result = sb.toString();
```

> [!tip] Why StringBuilder matters in DSA
> Building a string by repeated `s += ch` in a loop is **O(n²)** — each `+=` copies the whole string. Use `StringBuilder` (amortized O(1) append) then `toString()` → O(n). This single fix turns TLE into AC for many string problems.

## char <-> int
```java
char c = 'A';
int code = c;            // 65
char back = (char) 65;   // 'A'
c - 'a'                  // 0..25 letter index (lowercase)
Character.isDigit(c) / isLetter(c) / isLowerCase(c)
```

> [!warning] Pitfalls
> - `==` compares references for strings; always use `.equals()`.
> - `s.substring(i, j)` end is **exclusive**.
> - `String` is immutable → "modifying" creates garbage. For heavy edits use `StringBuilder` or `char[]`.

## Practice questions
- Check palindrome → [[Two-Pointers]] · [[Strings-DS]]
- Reverse words in a string → [[In-Place-Reversal]]
- Valid anagram → [[HashMap]]

## Related
- [[Strings-DS]] (DS view) · [[Input-Output]] · [[HashMap]]
