# **Number Theory**

### **1. Sieve of Eratosthenes**

The **Sieve of Eratosthenes** is a preprocessing technique that, in a single
pass over the range `[2, n]`, determines which numbers are prime and—if a
number is composite—records one of its prime divisors.

The algorithm maintains an array `sieve` indexed from 2 to *n*. A value of
`sieve[k] = 0` indicates that *k* is prime. If `sieve[k] ≠ 0`, then *k* is
composite, and the stored value is one of its prime factors.

Processing proceeds from 2 upward. Each time an index *x* is found to still
be unmarked (i.e., still prime), every multiple of *x* — namely `2x, 3x, 4x,
...` — is marked with the factor *x*, since *x* divides each of these numbers.

As an illustration, for *n* = 16 the table of smallest prime factors looks
like this (a 0 entry means the number itself is prime):

|  2 |  3 |  4 |  5 |  6 |  7 |  8 |  9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 |
|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
|  0 |  0 |  2 |  0 |  2 |  0 |  2 |  3 |  2 |  0 |  2 |  0 |  2 |  3 |  2 |

#### **Complexity Discussion**

For every prime *x* discovered, the marking step touches roughly `n / x`
cells. Summing this over all candidate values of *x* gives the harmonic-like
series:

∑ (n / x) for x = 2 .. n
= n/2 + n/3 + n/4 + ... + n/n

A naive bound on this sum is `O(n log n)`. However, since the inner marking
loop only runs when *x* is actually prime (composite values of *x* are
skipped because they were already marked), the true running time is much
smaller — it can be shown to be `O(n log log n)`, which is extremely close to
linear in *n*.

The implementations below assume the boolean array starts fully set to
`true` (i.e., everything is assumed prime until proven otherwise).

#### **C++ Implementation**

```cpp
vector<bool> sieve(int n) {
    vector<bool> isPrime(n + 1, true);
    isPrime[0] = isPrime[1] = false;
    for (int i = 2; (long long)i * i <= n; ++i) {
        if (isPrime[i]) {
            for (int j = i * i; j <= n; j += i)
                isPrime[j] = false;
        }
    }
    return isPrime;
}
```

#### **Python Implementation**

```python
def sieve(n):
    is_prime = [True] * (n + 1)
    is_prime[0] = is_prime[1] = False
    for i in range(2, int(n ** 0.5) + 1):
        if is_prime[i]:
            for j in range(i * i, n + 1, i):
                is_prime[j] = False
    return is_prime
```

#### **Java Implementation**

```java
boolean[] sieve(int n) {
    boolean[] isPrime = new boolean[n + 1];
    Arrays.fill(isPrime, true);
    isPrime[0] = isPrime[1] = false;
    for (int i = 2; (long) i * i <= n; i++) {
        if (isPrime[i]) {
            for (int j = i * i; j <= n; j += i) {
                isPrime[j] = false;
            }
        }
    }
    return isPrime;
}
```

---

### **2. Greatest Common Divisor and Least Common Multiple**

For two positive integers *a* and *b*, the **greatest common divisor**,
written `gcd(a, b)`, is the largest integer dividing both of them, while the
**least common multiple**, written `lcm(a, b)`, is the smallest positive
integer divisible by both. As an example, `gcd(48, 18) = 6` and
`lcm(48, 18) = 144`.

These two quantities are related through the identity:

```
lcm(a, b) = (a * b) / gcd(a, b)
```

**Euclid's algorithm** computes the GCD efficiently using the recursive rule:

```
gcd(a, b) =
    a                  if b = 0
    gcd(b, a mod b)    otherwise
```

Tracing through an example:

```
gcd(48, 18) = gcd(18, 12) = gcd(12, 6) = gcd(6, 0) = 6
```

#### **C++**

```cpp
int gcd(int a, int b) {
    return b == 0 ? a : gcd(b, a % b);
}
```

#### **Python**

```python
def gcd(a, b):
    return a if b == 0 else gcd(b, a % b)
```

#### **Java**

```java
int gcd(int a, int b) {
    return b == 0 ? a : gcd(b, a % b);
}
```

Euclid's algorithm runs in `O(log n)` time, where `n = min(a, b)`. The
slowest case occurs when *a* and *b* are consecutive Fibonacci numbers, since
each step reduces the pair to the next smaller Fibonacci pair, e.g.:

```
gcd(21, 13) = gcd(13, 8) = gcd(8, 5) = gcd(5, 3) = gcd(3, 2) = gcd(2, 1) = gcd(1, 0) = 1
```

---

### **3. Extended Euclidean Algorithm**

The **Extended Euclidean Algorithm** goes a step further than the standard
version: in addition to computing `g = gcd(a, b)`, it also finds integers
*x* and *y* satisfying **Bézout's identity**:

```
a * x + b * y = gcd(a, b)
```

This is particularly useful for solving linear Diophantine equations and for
computing modular inverses when the modulus is not necessarily prime.

#### **C++**

```cpp
int extendedGcd(int a, int b, int &x, int &y) {
    if (b == 0) {
        x = 1;
        y = 0;
        return a;
    }
    int x1, y1;
    int g = extendedGcd(b, a % b, x1, y1);
    x = y1;
    y = x1 - (a / b) * y1;
    return g;
}
```

#### **Python**

```python
def extended_gcd(a, b):
    if b == 0:
        return a, 1, 0
    g, x1, y1 = extended_gcd(b, a % b)
    x = y1
    y = x1 - (a // b) * y1
    return g, x, y
```

#### **Java**

```java
int extendedGcd(int a, int b, int[] xy) {
    if (b == 0) {
        xy[0] = 1;
        xy[1] = 0;
        return a;
    }
    int[] inner = new int[2];
    int g = extendedGcd(b, a % b, inner);
    xy[0] = inner[1];
    xy[1] = inner[0] - (a / b) * inner[1];
    return g;
}
```

---

### **4. Modular Exponentiation (Fast Power)**

Many problems require computing `x^n mod m` where *n* can be very large.
Computing this directly by repeated multiplication would take `O(n)`
multiplications, but the following identity allows it to be done in
`O(log n)`:

```
x^n mod m =
    (x * x)^(n/2) mod m       if n is even
    x * x^(n-1) mod m         if n is odd
```

The key observation is that whenever *n* is even, the base is squared once
and the exponent is halved, so the number of operations is proportional to
the number of bits in *n*.

#### **Worked Example**

To compute `3^13 mod 7`:

```
3^1  mod 7 = 3
3^2  mod 7 = 2
3^4  mod 7 = 4
3^8  mod 7 = 2
3^13 = 3^8 * 3^4 * 3^1 → (2 * 4 * 3) mod 7 = 24 mod 7 = 3
```

#### **C++**

```cpp
long long modPow(long long base, long long exp, long long mod) {
    long long result = 1;
    base %= mod;
    while (exp > 0) {
        if (exp & 1) result = (result * base) % mod;
        base = (base * base) % mod;
        exp >>= 1;
    }
    return result;
}
```

#### **Python**

```python
def mod_pow(base, exp, mod):
    result = 1
    base %= mod
    while exp > 0:
        if exp & 1:
            result = (result * base) % mod
        base = (base * base) % mod
        exp >>= 1
    return result
```

#### **Java**

```java
long modPow(long base, long exp, long mod) {
    long result = 1;
    base %= mod;
    while (exp > 0) {
        if ((exp & 1) == 1) result = (result * base) % mod;
        base = (base * base) % mod;
        exp >>= 1;
    }
    return result;
}
```

---

### **5. Modular Multiplicative Inverse**

The **modular inverse** of *a* with respect to modulus *m*, denoted `a⁻¹`,
is a number satisfying:

```
(a * a^-1) mod m = 1
```

For instance, with `a = 7` and `m = 11`, we have `a⁻¹ = 8`, since
`(7 * 8) mod 11 = 56 mod 11 = 1`.

An inverse does not always exist. For `a = 2` and `m = 4`, the equation
`(2x) mod 4 = 1` has no solution, because `2x` is always even and can never
leave a remainder of 1 when divided by 4. In general, `a⁻¹ mod m` exists if
and only if *a* and *m* are **coprime**, i.e., `gcd(a, m) = 1`.

#### **General Case — Using the Extended Euclidean Algorithm**

When `gcd(a, m) = 1`, applying the Extended Euclidean Algorithm to *a* and
*m* yields integers *x* and *y* such that `a*x + m*y = 1`. Taking this
equation modulo *m* shows that `a*x ≡ 1 (mod m)`, so *x* (reduced modulo *m*)
is the desired inverse.

#### **Special Case — Prime Modulus (Fermat's Little Theorem)**

When the modulus *p* is prime, Fermat's Little Theorem states:

```
a^(p-1) mod p = 1   (for a not divisible by p)
```

Multiplying both sides by `a⁻¹` and rearranging gives:

```
a⁻¹ = a^(p-2) mod p
```

This means the same fast modular exponentiation routine from Section 4 can
be reused directly to compute inverses when the modulus is prime.

#### **C++**

```cpp
// Requires m to be prime
long long modInverse(long long a, long long m) {
    return modPow(a, m - 2, m);
}
```

#### **Python**

```python
def mod_inverse(a, m):
    return mod_pow(a, m - 2, m)
```

#### **Java**

```java
// Requires m to be prime
long modInverse(long a, long m) {
    return modPow(a, m - 2, m);
}
```

---

### **6. Euler's Totient Function**

**Euler's totient function**, written `φ(n)`, counts how many integers in
the range `[1, n]` are coprime to *n*. For example, `φ(12) = 4`, since only
1, 5, 7, and 11 share no common factor with 12.

If the prime factorization of *n* is `p1^a1 * p2^a2 * ... * pk^ak`, then:

```
φ(n) = n * (1 - 1/p1) * (1 - 1/p2) * ... * (1 - 1/pk)
```

This function generalizes Fermat's Little Theorem (via Euler's theorem,
`a^φ(m) ≡ 1 (mod m)` whenever *a* and *m* are coprime) and is useful whenever
a problem requires counting coprime pairs or reducing exponents modulo a
composite number.

#### **Computing φ(n) for a single value — C++**

```cpp
long long phi(long long n) {
    long long result = n;
    for (long long p = 2; p * p <= n; ++p) {
        if (n % p == 0) {
            while (n % p == 0) n /= p;
            result -= result / p;
        }
    }
    if (n > 1) result -= result / n;
    return result;
}
```

#### **Computing φ(n) for a single value — Python**

```python
def phi(n):
    result = n
    p = 2
    while p * p <= n:
        if n % p == 0:
            while n % p == 0:
                n //= p
            result -= result // p
        p += 1
    if n > 1:
        result -= result // n
    return result
```

#### **Sieve-based totient table — useful when φ is needed for every value up to n**

```python
def totient_sieve(n):
    phi = list(range(n + 1))
    for p in range(2, n + 1):
        if phi[p] == p:  # p is prime
            for multiple in range(p, n + 1, p):
                phi[multiple] -= phi[multiple] // p
    return phi
```

---

## **Practice Problems**

### Easy

1. [GCD of an Array of Numbers – LeetCode](https://leetcode.com/problems/find-greatest-common-divisor-of-array/)
2. [Compute LCM and GCD – GeeksforGeeks](https://www.geeksforgeeks.org/problems/lcm-and-gcd/1)
3. [Primality Check – GeeksforGeeks](https://www.geeksforgeeks.org/problems/check-for-prime/1)
4. [List All Divisors of a Number – GeeksforGeeks](https://www.geeksforgeeks.org/problems/all-divisors-of-a-number/1)
5. [Numbers With Exactly Three Divisors – LeetCode](https://leetcode.com/problems/three-divisors/)

### Medium

1. [Implement the Sieve of Eratosthenes – GeeksforGeeks](https://www.geeksforgeeks.org/problems/sieve-of-eratosthenes5242/1)
2. [Sum of Primes in a Given Range – GeeksforGeeks](https://www.geeksforgeeks.org/sum-of-all-primes-in-a-given-range-using-sieve-of-eratosthenes/)
3. [Pow(x, n) — Fast Exponentiation – LeetCode](https://leetcode.com/problems/powx-n/)
4. [Multiplicative Inverse Under a Modulus – GeeksforGeeks](https://www.geeksforgeeks.org/multiplicative-inverse-under-modulo-m/)

### Hard

1. [Euler's Totient Function – GeeksforGeeks](https://www.geeksforgeeks.org/problems/totient-function/1)
2. [Fadi and LCM – Codeforces 1285C](https://codeforces.com/problemset/problem/1285/C)
3. [Remainder on Division by Powers of Two – Codeforces 913A](https://codeforces.com/problemset/problem/913/A)

---
