---
type: concept
tags:
  - java/syntax
  - java/operators
difficulty: easy
pattern: ""
related:
  - "[[Variables-and-Data-Types]]"
  - "[[Control-Flow]]"
  - "[[Bit-Manipulation]]"
aliases: []
---

# Operators

> [!note] Definition
> Operators produce values from operands. Precedence + associativity decide evaluation order; parentheses always win.

## Categories
| Family | Examples |
|--------|----------|
| Arithmetic | `+ - * / %` |
| Relational | `== != < > <= >=` |
| Logical | `&& \|\| !` (short-circuit) |
| Bitwise | `& \| ^ ~ << >> >>>` |
| Assignment | `= += -= *= /= %=` |
| Unary | `+ - ++ -- ! ~` |
| Ternary | `cond ? a : b` |

## Short-circuit evaluation
```java
if (a != 0 && b/a > 1) { ... }   // safe: right side skipped if a==0
if (s != null && s.length() > 0) // classic null guard
```
`&` and `|` (bitwise on booleans) evaluate **both** sides — no short-circuit.

## Increment/decrement
```java
int i = 5;
int a = i++;   // a=5, then i=6   (postfix: use old value)
int b = ++i;   // i=7, then b=7   (prefix: use new value)
```

## Bitwise (DSA gold)
```java
x & (x-1)        // clears lowest set bit -> power-of-2 check / popcount loop
x & -x           // isolates lowest set bit (Fenwick tree)
1 << k           // 2^k
n ^ n            // 0  (XOR self)
a ^ b ^ b        // a  (XOR is its own inverse -> missing-number trick)
```
> [!tip] See [[Bit-Manipulation]] for the full pattern toolkit.

## Ternary
```java
String sign = x >= 0 ? "non-negative" : "negative";
```

> [!warning] Pitfalls
> - `=` assigns, `==` compares. `if (a = b)` compiles if `b` is boolean.
> - `%` result sign follows the dividend: `-7 % 3 == -1`.
> - Mixing `int` with `long`: promote before overflow, e.g. `1L * a * b`.

## Related
- [[Variables-and-Data-Types]] · [[Bit-Manipulation]] · [[Control-Flow]]
