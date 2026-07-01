---
type: concept
tags:
  - dsa/algo/bit
difficulty: medium
pattern: ""
related:
  - "[[Operators]]"
  - "[[Variables-and-Data-Types]]"
  - "[[Math]]"
  - "[[Trie]]"
aliases:
  - Bit Manipulation
  - Bitwise
  - XOR tricks
---

# Bit Manipulation

> [!note] Definition
> Operating on the binary representation of integers with bitwise operators (`& | ^ ~ << >> >>>`). Two's complement: `-x == ~x + 1`. These tricks turn O(n) scans into O(1)/O(log w) operations and unlock XOR/counter problems.

## The essential identity kit
```java
x & (x-1)        // clears the LOWEST set bit       (popcount / power-of-2 test)
x & -x           // isolates the LOWEST set bit      (Fenwick tree)
x | (x+1)        // sets the lowest 0 bit
x ^ (x-1)        // isolates lowest set bit run
~x               // flips all bits
1 << k           // 2^k  (set bit k)
(x >> k) & 1     // read bit k
x | (1 << k)     // set bit k
x & ~(1 << k)    // clear bit k
x ^ (1 << k)     // toggle bit k
```

## Power-of-2 check
```java
boolean isPow2(int x){ return x > 0 && (x & (x-1)) == 0; }
```

## Popcount (number of set bits)
```java
int pop(int x){ int c=0; while (x!=0){ x &= x-1; c++; } return c; }
// or: Integer.bitCount(x)  (intrinsic, very fast)
```

## XOR — the superpower
XOR is associative, commutative, self-inverse (`a ^ a == 0`), identity `a ^ 0 == a`.
```java
// single number appearing once among pairs (all others twice)
int single(int[] a){ int r=0; for (int x:a) r ^= x; return r; }
// missing number 0..n: XOR all indices and all values
```
> [!tip] "Every element appears twice except one" → XOR everything; pairs cancel, the lone value remains. Generalizes: "all k times except one (k+1) times" needs counters, not plain XOR.

## Worked: count bits for every 0..n
```java
int[] countBits(int n){
    int[] dp = new int[n+1];
    for (int i = 1; i <= n; i++) dp[i] = dp[i >> 1] + (i & 1);   // reuse shifted answer
    return dp;
}
```
> [!tip] DP on bits: `dp[i] = dp[i >> 1] + (i & 1)` — the bit count of i equals that of i/2 plus the dropped LSB.

## Shifts and sign
- `<<` left shift (multiply by 2^k).
- `>>` **arithmetic** right shift (fills with sign bit) — preserves sign.
- `>>>` **logical** right shift (fills with 0) — unsigned.
- `>>> 1` on `Integer.MIN_VALUE` gives a large positive — useful to avoid overflow in mid calc `((a+b) >>> 1)`.

## Bitmask DP / enumeration
- Iterate all subsets of a mask: `for (int sub = mask; sub > 0; sub = (sub-1) & mask)` (descending).
- Iterate all masks 0..2^n → `for (int mask = 0; mask < (1<<n); mask++)`.
- Set/superset/subset sums (SOS DP).

## Bitwise trie (max XOR pair)
Build a trie of bit strings MSB→LSB; for each number, greedily pick the opposite bit at each level to maximize XOR. See [[Trie]].

> [!tip] Pattern recognition cues
> - "Only one element appears once, rest twice" → XOR.
> - "Count set bits / power of 2 / gray code" → bit tricks.
> - "Subset enumeration / small n ≤ 20 assignment" → bitmask DP ([[DP]]).
> - "Maximize XOR of a pair" → bitwise trie ([[Trie]]).
> - "Avoid overflow in midpoint" → `(lo + hi) >>> 1` ([[Searching]]).
> - "Toggle / presence flags cheaply" → a bitmask instead of `boolean[]`.

> [!warning] Pitfalls
  - Operator precedence: `&` and `|` bind **looser** than `==`! `x & 1 == 0` parses as `x & (1==0)`. Parenthesize: `(x & 1) == 0`.
  - Shifting by ≥32 (int) or ≥64 (long) is taken mod the word size → `1 << 32` is `1`, not 0. Use `long` and `1L << k` for k ≥ 31.
  - Signed vs unsigned right shift: `>>` vs `>>>` matters for negative numbers.
  - `~0` is `-1` (all ones), not 0 — to clear a bit use `& ~(1<<k)`, not `& (~0 - (1<<k))`.

## Practice questions
- [[Single-Number]] → XOR
- [[Number-of-1-Bits]] → popcount
- [[Counting-Bits]] → bit DP
- [[Reverse-Bits]] → shift/build
- [[Missing-Number]] → XOR / sum formula
- [[Maximum-XOR-of-Two-Numbers]] → bitwise trie

## Related
- [[Operators]] · [[Variables-and-Data-Types]] · [[Math]] · [[Trie]] · [[DP]]
