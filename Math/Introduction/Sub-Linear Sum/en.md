# Sublinear Sums

## Introduction  
In many problems, we need to compute the sum of a sequence in constant time $O(1)$ without iterating over all its terms. Such formulas are called **sublinear sums** because the running time does not grow linearly with $n$.

---

## Sum of the First $n$ Natural Numbers  
$$
S_1(n) = 1 + 2 + \cdots + n = \frac{n(n+1)}{2}
$$  
**Proof Sketch**: Pair terms $(1,n), (2,n-1), \dots$; each pair sums to $n+1$, and there are $n/2$ pairs.  

```cpp
// O(1)
long long sum_n(long long n) {
    return n * (n + 1) / 2;
}
```

---

## Sum of Squares  
$$
S_2(n) = 1^2 + 2^2 + \cdots + n^2 = \frac{n(n+1)(2n+1)}{6}
$$  

=== "c++"

```cpp
// O(1)
long long sum_sq(long long n) {
    return n * (n + 1) * (2*n + 1) / 6;
}
```

---

## Sum of Cubes  
$$
S_3(n) = 1^3 + 2^3 + \cdots + n^3 = \Bigl(\frac{n(n+1)}{2}\Bigr)^{2}
$$  

=== "c++"

```cpp
// O(1)
long long sum_cb(long long n) {
    long long t = n * (n + 1) / 2;
    return t * t;
}
```

---

## Sum of an Arithmetic Progression  
First term $a$, common difference $d$, $k$ terms:  
$$
S_{\rm AP} = a + (a+d) + \cdots + \bigl(a + (k-1)d\bigr)
= \frac{k\bigl(2a + (k-1)d\bigr)}{2}
$$  

=== "c++"

```cpp
// O(1)
long long sum_ap(long long a, long long d, long long k) {
    return k * (2*a + (k-1)*d) / 2;
}
```

---

## Sum of a Geometric Progression  
First term $a$, ratio $r \neq 1$, $k$ terms:  
$$
S_{\rm GP}
= a + ar + ar^2 + \cdots + ar^{k-1}
= a \,\frac{r^k - 1}{r - 1}
$$  

=== "c++"

```cpp
// O(1) assuming pow in O(log k)
long long sum_gp(long long a, long long r, long long k) {
    return a * ((long long)(std::pow(r, k) - 1) / (r - 1));
}
```

---

## Special Cases  
- **Sum of the first $n$ odd numbers**  
  $$
    1 + 3 + 5 + \cdots + (2n-1) = n^2
  $$
- **Sum of the first $n$ even numbers**  
  $$
    2 + 4 + 6 + \cdots + 2n = n(n+1)
  $$

---

## Overflow Considerations  
For large $n$ (e.g.\ $n>10^9$), intermediate products can exceed 64-bit limits. Use  
=== "c++"

```cpp
__int128
```  
for intermediate calculations, then cast back to `long long` if needed.

---

## Summary  
- All of the above formulas run in constant time $O(1)$.  
- They’re invaluable when you need repeated or very fast sum computations.  
- Keep multiplication before division to avoid truncation errors in integer arithmetic.
