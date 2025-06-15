### How to Check if a Number is Prime

Given a number $n$, how can we determine whether it is **prime** or **not**?

By definition, a number $n > 1$ is **prime** if it has no divisors other than $1$ and $n$.  
So, to check if a number is prime, we need to make sure that **no other number divides it**.

There are different ways to do this — from a simple brute-force approach to more optimized ones.

---

### Naive Prime Check – $O(n)$

The most basic method is to try dividing $n$ by every number from $2$ up to $n - 1$.

If any of those numbers divide $n$ exactly, then $n$ is **not** prime.

#### Pseudocode

```
If n is less than or equal to 1:
    Return false

For each number i from 2 to n - 1:
    If i divides n:
        Return false

Return true
```

#### Implementation

=== "c++"
```cpp
// Naive method to check if a number is prime
bool isPrimeNaive(int n) {
    if (n <= 1) return false;

    for (int d = 2; d < n; ++d) {
        if (n % d == 0) {
            // Found a divisor → not prime
            return false;
        }
    }

    return true; // No divisors found → prime
}
```

=== "Python"
```python
# Naive method to check if a number is prime
def is_prime_naive(n):
    if n <= 1:
        return False

    for d in range(2, n):
        if n % d == 0:
            # Found a divisor → not prime
            return False

    return True  # No divisors found → prime
```

---

### Optimized Prime Check – $O(\sqrt{n})$

We can significantly improve upon the naive approach by using a fundamental idea about divisors.

Suppose we are given a number $n$.  
If $n$ is **not a prime**, then by definition it must have a divisor **other than** $1$ and $n$ itself.  
Let’s call that divisor $d$.

Now, from the properties of divisors (as discussed earlier in **Introduction to number theory**), we know:

> Every divisor $d$ of $n$ appears with a corresponding pair $\frac{n}{d}$.  
> In any such pair, **at least one of the numbers must be ≤ \sqrt{n}**.

This gives us the key insight:

> If $n$ has a divisor other than 1 and itself, then the **smaller member of the divisor pair** must be $\le \sqrt{n}$.

So instead of checking every number from $2$ to $n - 1$, we only need to check up to **$\sqrt{n}$**.

If no number in that range divides $n$, then $n$ must be prime.

#### Pseudocode

```
If n is less than or equal to 1:
    Return false

For each number d from 2 to sqrt(n):
    If n % d == 0:
        Return false

Return true
```

#### Implementation

=== "c++"
```cpp
// Optimized method to check for primality using sqrt(n)
bool isPrime(int n) {
    if (n <= 1) return false;

    for (int d = 2; d * d <= n; ++d) {
        if (n % d == 0) {
            // Found a divisor → not prime
            return false;
        }
    }

    return true; // No divisors in [2, sqrt(n)] → prime
}
```

=== "Python"
```python
import math

# Optimized method to check for primality using sqrt(n)
def is_prime(n):
    if n <= 1:
        return False

    for d in range(2, int(math.isqrt(n)) + 1):
        if n % d == 0:
            # Found a divisor → not prime
            return False

    return True  # No divisors in [2, sqrt(n)] → prime
```

This optimization reduces the number of checks massively, especially for large numbers.