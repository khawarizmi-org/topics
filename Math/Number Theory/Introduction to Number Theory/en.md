# Introduction to Number Theory

**Number Theory** is a branch of mathematics dedicated to the study of integers and the relationships between them. It starts with simple ideas like divisibility and primes, but it leads to powerful techniques used in cryptography, algorithms, and computer science.

---


## What Is Modulo?

The **modulo operation** is a way of finding the **remainder** when one number is divided by another.  
It is written as `a % b`, and is read as “`a` mod `b`”.

### Example:
- `14 % 3 = 2` → because 14 divided by 3 is 4 with a remainder of 2  
- `12 % 4 = 0` → because 12 is exactly divisible by 4  
- `7 % 7 = 0` → every number is divisible by itself

Modulo is one of the core tools in number theory, it’s used to check if a number divides another (if `a % b == 0`)


---

### Introducing Congruence Notation

In number theory, we often express modulo relations more formally using **congruence** notation.

> Two integers `a` and `b` are said to be **congruent modulo `m`** if they leave the same remainder when divided by `m`.

Mathematically:
```
a ≡ b (mod m)  ⇔  (a - b) is divisible by m  ⇔  a % m == b % m
```

### Examples:

- `14 ≡ 2 (mod 3)` because both 14 and 2 leave remainder 2 when divided by 3
- `38 ≡ 2 (mod 6)` → since `38 - 2 = 36`, and 36 is divisible by 6
- `7 ≡ 0 (mod 7)` → every number is congruent to 0 modulo itself

This congruence relation helps us reason about divisibility and equivalence classes of integers under a given modulus. It’s also the foundation of modular arithmetic, which allows us to add, subtract, multiply, and even exponentiate **modulo m**.

---


## Divisors

Let `d` and `n` be positive integers.

> We say that `d` is a **divisor** of `n` if there exists an integer `k` such that `n = d × k`.

In other words, `d` divides `n` **exactly** — with **no remainder**.  
This is the same as saying:
```
n % d == 0
```

Every positive integer `n` has at least two divisors: `1` and `n` itself.  
We are often interested in finding **all divisors** of a given number `n`.

---

### Goal: Find All Divisors of `n`

We want a method that will list out **every number** that divides `n`.

---

### Naive Divisor Algorithm – O(n)

#### Pseudocode

```
For each number i from 1 to n:
    If n divided by i has no remainder:
        Then i is a divisor of n → add it to the list
```

This method checks **every number** up to `n`.

---

#### C++ Implementation

```cpp
#include <vector>

// Naive method: try all i from 1 to n and check if n % i == 0
std::vector<int> findDivisorsNaive(int n) {
    std::vector<int> divisors;

    for (int i = 1; i <= n; ++i) {
        if (n % i == 0) {
            // i is a divisor if it divides n with no remainder
            divisors.push_back(i);
        }
    }

    return divisors;
}
```

This works well for small `n`, but becomes inefficient as `n` grows — since it performs **n checks**.

---

### How Divisors Come in Pairs

Let `n` be a positive integer. A number `d` is called a **divisor** of `n` if there exists an integer `k` such that:

```
n = d × k
```

In this case, both `d` and `k` are divisors of `n`, since they multiply to `n`. This immediately tells us that **divisors come in pairs**: if `d` divides `n`, then `n / d` must also divide `n`.

We can think of every divisor `d` being paired with another divisor `n / d`. Therefore, all the divisors of `n` can be grouped into such **(d, n/d)** pairs.

---

### Why It’s Enough to Check Up to $\sqrt{n}$

Now we want to know: how many values of `d` do we need to check in order to find all the divisors?

Let’s consider two numbers `a` and `b` such that:

```
a × b = n
```

We claim that **at least one of `a` or `b` must be less than or equal to $\sqrt{n}$**.

#### Proof:

Assume for contradiction that both `a > $\sqrt{n}$` and `b > $\sqrt{n}$`.  
Then their product would satisfy:

```
a × b > $\sqrt{n}$ × $\sqrt{n}$ = n
```

But this contradicts our original assumption that `a × b = n`.  
Therefore, **it is impossible for both `a` and `b` to be greater than $\sqrt{n}$** if their product is `n`.

So in every divisor pair `(d, n/d)`, at least one of the numbers must be ≤ $\sqrt{n}$.  
This means:

> To find all divisors of `n`, it is sufficient to check for divisors `d` in the range `1 ≤ d ≤ $\sqrt{n}$`.  
> If `d` divides `n`, then `n / d` is automatically a valid divisor and can be added as its pair.

---

#### Example:

Let’s say `n = 36`.

If we check from `1` to `6` (since √36 = 6), we find:
- 1 → paired with 36
- 2 → paired with 18
- 3 → paired with 12
- 4 → paired with 9
- 6 → paired with 6 (only counted once)

Every divisor greater than `6` is just the result of `n / d` for some smaller divisor `d`.

This is why checking up to `$\sqrt{n}$` is not only sufficient but **optimal** for finding all divisors.


### Optimized Divisor Algorithm – O($\sqrt{n}$)

By using the insight above, we can reduce our work.  
We only iterate up to `$\sqrt{n}$` and for every divisor `i`, we add both `i` and `n / i`.

---

#### Pseudocode

```
For each number i from 1 to sqrt(n):
    If n is divisible by i:
        Add i to the list
        If n / i is different from i:
            Add n / i to the list
```

This way, we get both the small and large divisors efficiently.

---

#### C++ Implementation

```cpp
#include <vector>
#include <cmath>

// Optimized method: only check up to sqrt(n), and use divisor pairs
std::vector<int> findDivisors(int n) {
    std::vector<int> divisors;

    for (int i = 1; i * i <= n; ++i) {
        if (n % i == 0) {
            divisors.push_back(i);           // i is a divisor
            if (i != n / i) {
                divisors.push_back(n / i);   // n / i is the paired divisor
            }
        }
    }

    return divisors;
}
```

This approach reduces the time complexity from **O(n)** to **O($\sqrt{n}$)** and is the standard method in most number theory problems.

