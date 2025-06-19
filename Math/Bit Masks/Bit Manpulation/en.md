# Bit Manipulation

## Introduction
Bit manipulation refers to performing operations directly on the binary representations of integers. In competitive programming, bit tricks often yield $O(1)$ solutions to problems involving subsets, parity checks, power-of-two tests, and more.

## Binary Representation

Every non-negative integer can be represented as a sequence of bits (0s and 1s). Formally, a 32-bit unsigned integer $x$ has the binary form:

$$
 x = b_{31}2^{31} + b_{30}2^{30} + \dots + b_1 2^1 + b_0 2^0, \quad b_i \in \{0,1\}.
$$

Where:
- $b_i = 1$ means the bit at position $i$ (0-based, least significant) is set.
- The representation is often written as a fixed-width string, e.g., $x=13$ is $000\,00000\,00001101_2$ in 16-bit form.

Signed integers use two's complement: the most significant bit denotes sign, and negative numbers $-y$ are represented as $2^n - y$ in $n$-bit arithmetic.

## Basic Bitwise Operations
- **AND** (`&`): keeps bits set in both operands.  
  Example: `5 & 3 = 1` ($101_2 \& 011_2 = 001_2$).
- **OR** (`|`): sets bits that are set in either operand.  
  Example: `5 | 3 = 7` ($101_2 | 011_2 = 111_2$).
- **XOR** (`^`): sets bits that differ between operands.  
  Example: `5 ^ 3 = 6` ($101_2 \oplus 011_2 = 110_2$).
- **NOT** (`~`): flips all bits (two's complement representation).  
  Example: `~5 = -6` ($~101_2 = \dots111010_2$).
- **Left shift** (`<<`): shifts bits left, inserting zeros on the right, equivalent to multiplication by powers of two:  
  $x << k = x \times 2^k$.
- **Right shift** (`>>`): shifts bits right. For unsigned, inserts zeros; for signed, implementation-defined (often arithmetic shift):  
  $x >> k = \lfloor x / 2^k\rfloor$.

## Common Bit Tricks

### Extracting Lowest Set Bit
=== "c++"

```cpp
int lowbit(int x) {
    return x & -x;
}
```
Leverages two's complement: $-x = \overline{x} + 1$.  
Thus:  (
  \text{lowbit}(x) = x \& (-x).
)

### Clearing Lowest Set Bit
=== "c++"

```cpp
x &= (x - 1);
```
This removes the lowest set bit.  
Property: for $x > 0$,  $x \& (x - 1)$ clears the rightmost `1`-bit.

### Checking Power of Two
=== "c++"

```cpp
bool isPowerOfTwo(int x) {
    return x > 0 && (x & (x - 1)) == 0;
}
```
A number is a power of two iff it has exactly one `1`-bit.  
Equivalently:  $x > 0$ and $x \& (x - 1) = 0$.

### Counting Bits (Population Count)
Use GCC built-in:
=== "c++"

```cpp
int cnt = __builtin_popcount(x);       // for 32-bit
int cnt64 = __builtin_popcountll(x);  // for 64-bit
```
Time complexity is $O(1)$ at the hardware level.

## Iterating Over Submasks
Given mask $S$, iterate all non-empty submasks:
=== "c++"

```cpp
for (int sub = S; sub; sub = (sub - 1) & S) {
    // process sub
}
```
This runs in $O(3^n)$ over all subsets of an $n$-bit mask, summed across all masks.

## Iterating Over All Subsets

For an $n$-element set, label elements from 0 to $n-1$. Each subset corresponds to a bitmask $m$ ranging from 0 to $2^n - 1$:
=== "c++"

```cpp
int total = 1 << n;
for (int mask = 0; mask < total; ++mask) {
    vector<int> subset;
    for (int i = 0; i < n; ++i) {
        if (mask & (1 << i))
            subset.push_back(i);
    }
    // process subset
}
```
This loops through all $2^n$ subsets in $O(n2^n)$ time.

## Applications in CP
- **Bitmask DP**: states encoded as masks, e.g., TSP in $O(n^2 2^n)$.
- **Subset enumeration**: meet-in-the-middle, SOS DP.
- **Parity and Signing**: computing parity $x$-bits mod 2 via XOR.
- **Fast Multiplications**: check even/odd, mod by power-of-two:  $x \bmod 2^k = x & (2^k - 1)$.

## C++ Built-in Bit Operations

C++ provides a set of built-in functions for common bit operations, all running in constant time on most platforms.

### Population Count and Bit Parity
- `int cnt = __builtin_popcount(x);` // number of `1`-bits in a 32-bit integer.
- `int cnt64 = __builtin_popcountll(x);` // number of `1`-bits in a 64-bit integer.
- `int pr = __builtin_parity(x);` // parity (sum of bits mod 2).

### Count Leading and Trailing Zeros
- `int lz = __builtin_clz(x);` // number of leading zeros in 32-bit `x` (undefined if x==0).
- `int lz64 = __builtin_clzll(x);` // leading zeros in 64-bit.
- `int tz = __builtin_ctz(x);` // number of trailing zeros in 32-bit `x` (undefined if x==0).
- `int tz64 = __builtin_ctzll(x);` // trailing zeros in 64-bit.

### Find First Set Bit
- `int f = __builtin_ffs(x);` // index (1-based) of least significant `1`-bit, or 0 if x==0.

These built-ins are invaluable for fast bit-level queries and are widely supported in GCC, Clang, and compatible compilers.

## Complexity and Best Practices
- All basic bit operations (`&`, `|`, `^`, `~`, `<<`, `>>`) run in $O(1)$.
- Use built-ins for popcount and clz (count leading zeros):  
=== "c++"

  ```cpp
  int leading = __builtin_clz(x);
  ```
- Avoid relying on signed right-shift behavior across compilers.

## Summary
Bit manipulation offers elegant, constant-time solutions to many problems. Mastering these tricks can greatly speed up implementation and reduce time complexity in competitive programming.
