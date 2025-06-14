# Introduction to Number Theory

**Number Theory** is a branch of mathematics dedicated to the study of integers and the relationships between them. It starts with simple ideas like divisibility and primes, but it leads to powerful techniques used in cryptography, algorithms, and computer science.

---

## Divisibility

The central concept in number theory is **divisibility**.

Consider the integers $\mathbb{Z} = \{\dots, -2, -1, 0, 1, 2, \dots \}$, for $a, b, \in \mathbb{Z}$, we say that $a$ **divides** $b$ if $a \dot x = b$ for some $ x \in \mathbb{Z}$. If $a$ divides $b$ we write $a \divides b$, and we say that $a$ is a **divisor** of $b$, or that $b$ is **multiple** of $a$ or that $b$ is **divisible by** $a$. If $a$ does not divide $b$, then we write $a \notdivides b$.

### Theorem
For all $a, b, c, \in \mathbb{Z}$, we have:
- $a \divides a$, $1 \divides a$ and $a \divides 0$.
- $a \divides b$ **if and only if** $-a \divides a$ **if and only if** $a \divides -b$.
- $a \divides b$ and $a \divides c$ implies $a \divides (b + c)$.
- $a \divides b$ and $a \divides c$ implies $a \divides c$.
- $a \divides b$ and $b \divides a$ **if and only if ** $a = \pm b$.

## What Is Modulo?

The result of the **modulo operation** is the **remainder** when one number is divided by another.  
In **mathematics**, this is written as:

$$
a mod b
$$

and is read as “**$a \bmod b$**”.

In **programming**, the modulo operation is commonly represented using the percent sign $%$, so it appears as:

```cpp
a % b
```

### Example:
- $14$ \bmod $3$ = $2$ $\rightarrow$ because $14$ divided by $3$ is $4$ with a remainder of $2$  
- $12$ \bmod $4$ = $0$ $\rightarrow$ because $12$ is exactly divisible by $4$  
- $7$ \bmod $7$ = $0$ $\rightarrow$ every number is divisible by itself

Modulo is one of the core tools in number theory, it’s used to check if a number divides another (if $a$ % $b$ == $0$)


---

### Introducing Congruence Notation

In number theory, we often express modulo relations more formally using **congruence** notation.

> Two integers $a$ and $b$ are said to be **congruent modulo $m$** if they leave the same remainder when divided by $m$.

Mathematically:
$$
$a \equiv b\ \pmod{m} \Leftrightarrow (a - b)$ is divisible by $m \Leftrightarrow a \bmod m = b \bmod m$
$$

### Examples:

- $14 \equiv 2 (mod 3)$ because both $14$ and $2$ leave remainder $2$ when divided by $3$
- $38 \equiv 2 (mod 6)$ $\rightarrow$ since $38$ - $2$ = $36$, and $36$ is divisible by $6$
- $7 \equiv 0 (mod 7)$ $\rightarrow$ every number is congruent to $0$ modulo itself

This congruence relation helps us reason about divisibility and equivalence classes of integers under a given modulus. It’s also the foundation of modular arithmetic, which allows us to add, subtract, multiply, and even exponentiate **modulo $m$**.

---

## Divisors

Let $d$ and $n$ be positive integers.

> We say that $d$ is a **divisor** of $n$ if there exists an integer $k$ such that $n = d \times k$.

In other words, $d$ divides $n$ **exactly** — with **no remainder**.  
This is the same as saying:
$$
n \bmod d = 0
$$

Every positive integer $n$ has at least two divisors: $1$ and $n$ itself.  
We are often interested in finding **all divisors** of a given number $n$.

---

### Goal: Find All Divisors of $n$

We want a method that will list out **every number** that divides $n$.

---

### Naive Divisor Algorithm – $O(n)$

#### Pseudocode

```
For each number i from 1 to n:
    If n divided by i has no remainder:
        Then i is a divisor of n → add it to the list
```

This method checks **every number** up to $n$.

---

#### C++ Implementation

```cpp
#include <vector>
using namespace std;

// Naive method: try all i from 1 to n and check if n % i == 0
vector<int> findDivisorsNaive(int n) {
    vector<int> divisors;

    for (int i = 1; i <= n; ++i) {
        if (n % i == 0) {
            // i is a divisor if it divides n with no remainder
            divisors.push_back(i);
        }
    }

    return divisors;
}
```

This works well for small $n$, but becomes inefficient as $n$ grows — since it performs **$n$ checks**.

---

### How Divisors Come in Pairs

Let $n$ be a positive integer. A number $d$ is called a **divisor** of $n$ if there exists an integer $k$ such that:

$$
n = d \times k
$$

In this case, both $d$ and $k$ are divisors of $n$, since they multiply to $n$. This immediately tells us that **divisors come in pairs**: if $d \divides n$, then $\frac{n}{d} \divide n$ also holds.

We can think of every divisor $d$ being paired with another divisor $\frac{n}{d}$. Therefore, all the divisors of $n$ can be grouped into such **($d$, $\frac{n}{d}$)** pairs.

---

### Why It’s Enough to Check Up to $\sqrt{n}$

Now we want to know: how many values of $d$ do we need to check in order to find all the divisors?

Let’s consider two numbers $a$ and $b$ such that:

$$
a \times b = n
$$

We claim that **at least one of $a$ or $b$ must be less than or equal to $\sqrt{n}$**.

#### Proof:

Assume for contradiction that both $a > \sqrt{n}$ and $b > \sqrt{n}$.  
Then their product would satisfy:

$$
a \times b > \sqrt{n} \times \sqrt{n} = n
$$

But this contradicts our original assumption that $a \times b = n$.  
Therefore, **it is impossible for both $a$ and $b$ to be greater than $\sqrt{n}$** if their product is $n$.

So in every divisor pair $(d, \frac{n}{d})$, at least one of the numbers must be $\le \sqrt{n}$.  
This means:

> To find all divisors of $n$, it is sufficient to check for divisors $d$ in the range $1 \le d \le \sqrt{n}$.  
> If $d \divides n$, then $\frac{n}{d}$ is automatically a valid divisor and can be added as its pair.

---

#### Example:

Let’s say $n = 36$.

If we check from $1$ to $6$ (since $\sqrt{36}$ = $6$), we find:
- $1$ $\rightarrow$ paired with $36$
- $2$ $\rightarrow$ paired with $18$
- $3$ $\rightarrow$ paired with $12$
- $4$ $\rightarrow$ paired with $9$
- $6$ $\rightarrow$ paired with $6$ (only counted once)

Every divisor greater than $6$ is just the result of $\frac{n}{d}$ for some smaller divisor $d$.

This is why checking up to $\sqrt{n}$ is not only sufficient but **optimal** for finding all divisors.


### Optimized Divisor Algorithm – $O(\sqrt{n})$

By using the insight above, we can reduce our work.  
We only iterate up to $\sqrt{n}$ and for every divisor $i$, we add both $i$ and $n / i$.

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

This approach reduces the time complexity from **$O(n)$** to **$O(\sqrt{n})$** and is the standard method in most number theory problems.

## Prime Numbers

In number theory, one of the most fundamental types of numbers is the **prime number**.

A **prime number** is a natural number greater than $1$ that has **exactly two distinct positive divisors**:

- $1$, and  
- the number itself.

This means that you cannot write a prime number as a product of two smaller natural numbers, other than trivially as $1 \times n$.


---

### Examples of Prime and Non-Prime Numbers

Let’s look at a few examples to see the difference between prime and composite numbers:

- $2$ $\rightarrow$ $\textcolor{green}{prime}$
    - Divisors: $1$ and $2$
- $3$ $\rightarrow$ $\textcolor{green}{prime}$
    - Divisors: $1$ and $3$
- $4$ $\rightarrow$ $\textcolor{red}{Not prime}$ (composite)
    - Divisors: $1$, $2$, $4$ $\rightarrow$ more than two
- $5$ $\rightarrow$ $\textcolor{green}{prime}$
    - Divisors: $1$ and $5$
- $6$ $\rightarrow$ $\textcolor{red}{Not prime}$ (composite)
    - Divisors: $1$, $2$, $3$, $6$
- $7$ $\rightarrow$ $\textcolor{green}{prime}$
    - Divisors: $1$ and $7$
- $8$ $\rightarrow$ $\textcolor{red}{prime}$ (composite)
    - Divisors: $1$, $2$, $4$, $8$
- $9$ $\rightarrow$ $\textcolor{red}{Not prime}$ (composite)
    - Divisors: $1$, $3$, $9$
- $11$ $\rightarrow$ $\textcolor{green}{prime}$
    - Divisors: $1$ and $11

---

### Special Notes:

- $1$ is **not** a prime number.  
  - It only has one divisor ($1$ itself), so it doesn’t satisfy the definition.
- The **smallest prime** is $2$.
  - It’s also the **only even prime** — every other even number is divisible by $2$ and thus composite.

---