---
type: concept
tags:
  - dsa/algo/math
difficulty: medium
pattern: ""
related:
  - "[[Bit-Manipulation]]"
  - "[[DP]]"
  - "[[Recursion]]"
aliases:
  - Math
  - Number theory
  - GCD
  - Primes
---

# Math

> [!note] Definition
> Algorithmic math: number theory (GCD/LCM, modular arithmetic, primes, factorization), combinatorics (nCr, factorials), and basic geometry. Frequent in CP and in "trick" interview questions where the O(n) loop is replaced by an O(1) formula.

## GCD & LCM
```java
long gcd(long a, long b){ while (b != 0){ long t = a % b; a = b; b = t; } return a; }
long lcm(long a, long b){ return a / gcd(a, b) * b; }   // divide first to avoid overflow
```
Euclid's algorithm: O(log min(a,b)). LCM formula: `a·b / gcd(a,b)`.

## Modular arithmetic
```java
final long MOD = 1_000_000_007L;
long add(long a, long b){ return (a + b) % MOD; }
long mul(long a, long b){ return (a * b) % MOD; }            // careful: a*b may overflow if a,b up to MOD
long mul(long a, long b){ return ((a % MOD) * (b % MOD)) % MOD; }
// modular exponentiation (binary exponentiation), O(log e)
long pow(long b, long e, long m){
    long r = 1; b %= m;
    while (e > 0){ if ((e & 1) == 1) r = r * b % m; b = b * b % m; e >>= 1; }
    return r;
}
// modular inverse (Fermat), only if m is prime: a^(m-2) mod m
long inv(long a, long m){ return pow(a, m-2, m); }
```
> [!warning] `a * b % MOD` overflows if `a, b` are up to MOD (~1e9) since product ~1e18 fits in long but `a*b` of two longs each < MOD is fine; just reduce first. Multiplying two full MOD-range `long`s is fine (< 9.2e18). But multiplying values up to 1e18 needs 128-bit or modular mul tricks.

## Primes
**Trial division** (test up to √n):
```java
boolean isPrime(long n){
    if (n < 2) return false;
    for (long d = 2; d * d <= n; d++) if (n % d == 0) return false;
    return true;
}
```
**Sieve of Eratosthenes** (all primes ≤ n), O(n log log n):
```java
boolean[] sieve(int n){
    boolean[] prime = new boolean[n+1]; Arrays.fill(prime, true);
    prime[0]=prime[1]=false;
    for (int i = 2; (long)i*i <= n; i++) if (prime[i])
        for (int j = i*i; j <= n; j += i) prime[j] = false;
    return prime;
}
```
**Prime factorization** (trial division by 2 then odd up to √n): O(√n) per number.

## Combinatorics
```java
// nCr mod p using factorials + inverses (precompute fact[] and invFact[])
long nCr(int n, int r){ return fact[n] * invFact[r] % MOD * invFact[n-r] % MOD; }
// Pascal's triangle for small n: C[n][r] = C[n-1][r-1] + C[n-1][r]
```
- Permutations: `P(n, r) = n! / (n-r)!`.
- Catalan numbers: `C_k = (2k choose k) / (k+1)` (valid parentheses, BST shapes).

## Sum formulas (O(1) replaces O(n))
- 1 + 2 + … + n = `n(n+1)/2`
- 1² + … + n² = `n(n+1)(2n+1)/6`
- Sum of AP: `n·(first + last)/2`.
- Sum of GP: `a·(r^n − 1)/(r − 1)`.
> [!tip] [[Missing-Number]] and "sum vs expected" problems → use the closed form, not a loop.

## Fast exponentiation (non-modular)
```java
long pow(long b, long e){ long r=1; while(e>0){ if((e&1)==1) r*=b; b*=b; e>>=1; } return r; }
```
O(log e). Used for power-of computations and as the backbone of modular pow.

## Geometry basics
- Distance: `Math.hypot(dx, dy)`.
- Triangle area (shoelace): `|Σ (x_i·y_{i+1} − x_{i+1}·y_i)| / 2`.
- Cross product sign = orientation (left/right turn / collinear) — basis for convex hull (Andrew's monotone chain).
- Dot product = projection / angle.

> [!tip] Pattern recognition cues
> - "Repeatedly halve / multiply by power" → binary exponentiation.
> - "All pairs gcd / lcm" → factor + frequency, or `gcd` properties.
> - "Count primes / prime factors up to n" → sieve.
> - "nCr / number of ways" → precomputed factorials + modular inverse.
> - "Sum 1..n / arithmetic series" → closed formula.
> - "Points / orientation / hull" → cross-product geometry.

> [!warning] Pitfalls
  - Integer overflow in `n(n+1)/2` for large n → use `long`.
  - In `lcm`, multiply **after** dividing by gcd to avoid overflow.
  - Sieve `i*i` overflow: cast to long in the loop bound.
  - Modular inverse via Fermat requires **prime** modulus; for composite use extended Euclid.
  - Floating-point for exact integer problems → accumulate rounding error; use integer math.

## Practice questions
- [[Missing-Number]] → sum formula / XOR
- [[Pow-x-n]] → binary exponentiation
- [[Count-Primes]] → sieve
- [[Roman-to-Integer]] / [[Integer-to-Roman]] → mapping
- [[Happy-Number]] → cycle detection ([[Fast-Slow-Pointers]]) + digit squares

## Related
- [[Bit-Manipulation]] (binary expo, XOR) · [[DP]] (Pascal, Catalan) · [[Recursion]]
