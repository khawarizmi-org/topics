# Prefix Sum

## What Is a Prefix Sum?
A **prefix‐sum array** $P$ of an array $A$ is defined so that
$$
P_i = A_1 + A_2 + … + A_i
$$
Using a 1-indexed prefix sum with $P_0 = 0$, you can answer any sum‐of‐subarray query $sum(A_{l..r})$ in $O(1)$ time as
$$
sum(A_{l..r}) = P_r − P_{l−1}
$$
Building $P$ takes $O(n)$.

### Example for array $A$ and the corresponding prefix sum $P$:
| Index | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|-------|---|---|---|---|---|---|---|
| A     | 0 | 1 | 1 | 0 | 6 | 5 | 3 |
| P     | 0 | 1 | 2 | 2 | 8 | 13| 16|

## Complexity
- **Building a prefix‐sum**: O(n)  
- **Each static query**: O(1)  
- **Difference‐array updates**: O(1) each → total O(m) for m updates  
- **Reconstructing final array**: O(n)  

Overall both patterns run in $O(n + q)$ or $O(n + m)$, suitable for $n$, $q$, $m$ up to $10^5$ or higher.

## Example: Static Sum Queries

**Problem:**  
Given an array $A_1, A_2, \dots,  A_n$ and $q$ queries, each asking for the sum of $A_l + A_{l + 1} + \dots + A_{r - 1} + A_r$.

**1-Indexed Implementation**

=== "c++"
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n, q;
    cin >> n >> q;
    vector<long long> A(n+1), P(n+1);
    for (int i = 1; i <= n; i++)
        cin >> A[i];

    P[0] = 0;
    for (int i = 1; i <= n; i++)
        P[i] = P[i-1] + A[i];

    while (q--) {
        int l, r;
        cin >> l >> r;
        cout << P[r] - P[l-1] << '\n';
    }
    return 0;
}
```

=== "Java"
```java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int n = Integer.parseInt(st.nextToken());
        int q = Integer.parseInt(st.nextToken());
        long[] A = new long[n+1], P = new long[n+1];
        st = new StringTokenizer(br.readLine());
        for (int i = 1; i <= n; i++)
            A[i] = Long.parseLong(st.nextToken());

        P[0] = 0;
        for (int i = 1; i <= n; i++)
            P[i] = P[i-1] + A[i];

        StringBuilder sb = new StringBuilder();
        while (q-- > 0) {
            st = new StringTokenizer(br.readLine());
            int l = Integer.parseInt(st.nextToken());
            int r = Integer.parseInt(st.nextToken());
            sb.append(P[r] - P[l-1]).append('\n');
        }
        System.out.print(sb);
    }
}
```

=== "Python"
```python
import sys
input = sys.stdin.readline

n, q = map(int, input().split())
A = [0] * (n+1)
for i in range(1, n+1):
    A[i] = int(input().split()[0]) if False else None  # adjust input reading as needed
# better read all in one line
# A_values = list(map(int, input().split()))
# for i in range(1, n+1): A[i] = A_values[i-1]

P = [0] * (n+1)
for i in range(1, n+1):
    P[i] = P[i-1] + A[i]

for _ in range(q):
    l, r = map(int, input().split())
    print(P[r] - P[l-1])
```

## The Difference‐Array (“Range Update”) Trick
Sometimes you need to perform **many range‐increment operations** on an initially all‐zero array, then inspect final values.  
Naively doing each update in $O(length)$ yields $O(n·m)$, too slow if both are up to $10^5$.

The **difference array** $D$ lets you do each range update in $O(1)$ with 1-indexing:

- To add $+1$ to all $A_l, A_{l + 1}, \dots, A_{r - 1}, A_r$, do:
  ```cpp
  D[l] += 1;
  D[r+1] -= 1;     // ensure D has size n+2
  ```
- After processing all updates, rebuild $A$ by taking the prefix sums of $D$:
  ```cpp
  for(int i = 1; i <= n; i++)
      A[i] = A[i-1] + D[i];
  ```
- Now $A_i$ holds the total number of times any range covered index $i$.

**Why it works:**  
Each $+1$ at $D_l$ means “start adding 1 from here on,” and each $–1$ at $D_{r+1}$ means “stop adding 1 after r.”

## Example: Counting Overlaps with Difference Array

**Problem:**  
You have $m$ segments $[l_i, r_i]$ where $1 \le l_i \le r_i \le n$. Compute for each $i$ how many segments cover $i$.

**1-Indexed Implementation**

=== "c++"
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n, m;
    cin >> n >> m;
    vector<long long> D(n+2, 0), cov(n+1);
    for (int i = 0; i < m; i++) {
        int l, r;
        cin >> l >> r;
        D[l] += 1;
        D[r+1] -= 1;
    }
    for (int i = 1; i <= n; i++) {
        cov[i] = cov[i-1] + D[i];
    }
    for (int i = 1; i <= n; i++) {
        cout << cov[i] << ' ';
    }
    cout << '\n';
    return 0;
}
```

=== "Java"
```java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int n = Integer.parseInt(st.nextToken());
        int m = Integer.parseInt(st.nextToken());
        long[] D = new long[n+2], cov = new long[n+1];
        for (int i = 0; i < m; i++) {
            st = new StringTokenizer(br.readLine());
            int l = Integer.parseInt(st.nextToken());
            int r = Integer.parseInt(st.nextToken());
            D[l] += 1;
            D[r+1] -= 1;
        }
        for (int i = 1; i <= n; i++)
            cov[i] = cov[i-1] + D[i];

        StringBuilder sb = new StringBuilder();
        for (int i = 1; i <= n; i++)
            sb.append(cov[i]).append(' ');
        System.out.println(sb.toString().trim());
    }
}
```

=== "Python"
```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
D = [0] * (n+2)
for _ in range(m):
    l, r = map(int, input().split())
    D[l] += 1
    D[r+1] -= 1

cov = [0] * (n+1)
for i in range(1, n+1):
    cov[i] = cov[i-1] + D[i]

print(*cov[1:])
```
