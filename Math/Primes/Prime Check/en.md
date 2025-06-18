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

```plaintext
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
> In any such pair, **at least one of the numbers must be $\le \sqrt{n}$**.

This gives us the key insight:

> If $n$ has a divisor other than 1 and itself, then the **smaller member of the divisor pair** must be $\le \sqrt{n}$.

So instead of checking every number from $2$ to $n - 1$, we only need to check up to **$\sqrt{n}$**.

If no number in that range divides $n$, then $n$ must be prime.

#### Pseudocode

```plaintext
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

## Finding All Primes from $1$ to $n$

We often want to generate **all primes up to $n$**, especially in algorithmic problems.

---

### Brute Force Approach – $O(n\sqrt{n})$

This method is based on repeatedly using the **prime check** function for each number from $2$ to $n$.

It’s simple and intuitive but becomes inefficient for large $n$.

#### How It Works

For each integer $i$ from $2$ to $n$:
- Check whether $i$ is a prime using `isPrime()` function.
- If it is prime, add it to the list of primes.

Since `isPrime()` takes $O(\sqrt{n})$, and we call it $n$ times, the total complexity is:

$$
O(n\sqrt{n})
$$

#### Pseudocode

```plaintext
Initialize an empty list called primes

For every number i from 2 to n:
    If i is prime:
        Add i to the primes list

Return the list of primes
```

#### Implementation

=== "c++"
```cpp
#include <vector>
using namespace std;

vector<int> getPrimesSlow(int n) {
    vector<int> primes;

    for (int i = 2; i <= n; ++i) {
        if (isPrime(i)) {
            primes.push_back(i);
        }
    }

    return primes;
}
```

=== "Python"
```python

def get_primes_slow(n):
    primes = []

    for i in range(2, n + 1):
        if is_prime(i):
            primes.append(i)

    return primes
```

---

### Sieve of Eratosthenes – $O(n \cdot \log(\log(n)))$

This is one of the **most efficient algorithms** for generating **all primes ≤ $n$**.

Rather than checking each number individually, the Sieve eliminates **non-prime numbers in bulk** by exploiting their multiplicative structure.

### How it Works

1. **Start with all numbers marked as prime**.
2. **Loop from $2$ to $n$**.
3. **If $i$ is still marked as prime**:
   - Then $i$ is guaranteed to be prime.
   - Mark **all multiples of $i$** (i.e., $2i, 3i, 4i, ...$) as not prime.

### Why Is the Sieve of Eratosthenes $O(n \cdot \log(\log(n)))$?

To analyze the time complexity of the sieve, we look at how many times each composite number gets marked as **not prime**.

Let’s consider the **total number of operations** — that is, how many times we iterate over any multiple of a prime.

So for every prime $p \leq n$, we mark off:

$$
\left\lfloor \frac{n}{p} \right\rfloor - 1 \quad \text{multiples (excluding p itself)}
$$

So the total number of operations is:

$$
\sum_{p \leq n,\ p\ \text{prime}} \left\lfloor \frac{n}{p} \right\rfloor
$$

We can estimate this by:

$$
\sum_{p \leq n} \frac{n}{p} = n \cdot \sum_{p \leq n} \frac{1}{p}
$$

The term $\sum_{p \leq n} \frac{1}{p}$ is a well-known mathematical expression — the **harmonic sum over primes** — and it grows like:

$$
\sum_{p \leq n} \frac{1}{p} \approx \log(\log(n))
$$

Therefore, the total number of operations is approximately:

$$
n \cdot \log(\log(n))
$$

### Implementation

=== "c++"
```cpp
#include <vector>
include namespace std;

// Classic sieve algorithm to generate primes up to n
vector<int> sieve(int n) {
    vector<bool> isPrime(n + 1, true); // initially assume all prime
    isPrime[0] = isPrime[1] = false;

    for (int p = 2; p * p <= n; ++p) {
        if (isPrime[p]) {
            // mark multiples of p starting from p*p
            for (int multiple = p * p; multiple <= n; multiple += p) {
                isPrime[multiple] = false;
            }
        }
    }

    vector<int> primes;
    for (int i = 2; i <= n; ++i) {
        if (isPrime[i]) {
            primes.push_back(i);
        }
    }

    return primes;
}
```

=== "Python"
```python
def sieve_to_n(n):
    is_prime = [True] * (n + 1)
    is_prime[0] = is_prime[1] = False

    for i in range(2, n + 1):
        if is_prime[i]:
            # Mark all multiples of i as not prime
            for multiple in range(2 * i, n + 1, i):
                is_prime[multiple] = False

    primes = [i for i, prime in enumerate(is_prime) if prime]
    return primes
```