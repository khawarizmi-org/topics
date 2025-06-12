### Meet in the Middle

**Meet-in-the-Middle** is a classic **time–space trade-off** technique that can turn an $O(k^n)$ brute-force search into roughly $O(k^{n/2})$ by splitting the problem into two halves, solving each half independently, and then merging the results efficiently.

#### Example Problem

You are given an integer array $a$ of length $n$ and an integer $k$. Determine whether there exists a subsequence of $a$ whose sum equals $k$.

Constraints:

$$
1 \le n \le 40,\quad
1 \le a_i \le 10^9,\quad
1 \le k \le 4 \cdot 10^{10}.
$$

A naive complete search examines all $2^n$ subsequences in $O(2^n)$ time, which is too large for $n=40$.

---

#### Meet in the middle technique:

We can split the array into two equal halves, and find the sum of all subsequences for each half and store the result.

Since the size of the array is $n$, the size of each half is $n/2$, so the number of subsequences for each half is $2^{\frac{n}{2}}$.

For example if we have $a = [1, 5, 2, 4]$, we can split it into two halves $h1 = [1, 5]$ and $h2 = [2, 4]$, then we can find the summation of all subsequences for each half.

Now for each half let's store the sum of all subsequences into two arrays $s1$ and $s2$.

So in the previous example $s1 = [0, 1, 5, 6]$, and $s2 = [0, 2, 4, 6]$ ($0$ comes from the empty subsequence).

A subsequence from the original array $a$, will have some elements from the first half, and some elements from the second half, so its sum will be an element from $s1$ + an element from $s2$.

So the problem now is to find two indices $i$ and $j$ so that $s1_i + s2_j = k$.

We can do this by iterating over the array $s1$, and for each $i$ $(1 \leq i \leq |s1|)$ we need to check if there exists an element in $s2$ that is equal to $k - s1_i$, we can do this by sorting $s2$ then binary search on each value.

The total complexity becomes $O\bigl(2^{\frac{n}{2}} \cdot \log{2^{\frac{n}{2}}}\bigr)$ which is $O\bigl(2^{\frac{n}{2}} \cdot \frac{n}{2}\bigr)$, which is accepted under the given constraints.

#### code:

=== "c++"

```c++
#include <vector>
#include <algorithm>

void sum_of_all_subsets(const vector<int> &a, int i, int sum, vector<long long> &res){
    if (i == (int)a.size()){
        res.push_back(sum);
        return;
    }
    sum_of_all_subsets(a, i + 1, sum, res);
    sum_of_all_subsets(a, i + 1, sum + a[i], res);
}

bool has_subset_with_sum(vector<int> a, int n, long long k){
    vector<int> h1, h2;
    for (int i = 0; i < n / 2; i++)
        h1.push_back(a[i]);
    for (int i = n / 2; i < n; i++)
        h2.push_back(a[i]);
    vector<long long> s1, s2;
    sum_of_all_subsets(h1, 0, 0, s1);
    sum_of_all_subsets(h2, 0, 0, s2);

    sort(s2.begin(), s2.end());

    for (int i = 0; i < (int)s1.size(); i++){
        if (binary_search(s2.begin(), s2.end(), k - s1[i])){
            return true;
        }
    }
    return false;
}
```

=== "Java"

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class MeetInMiddle {

    private static void generateSubsetSums(List<Integer> a, int idx, long sum, List<Long> res) {
        if (idx == a.size()) {
            res.add(sum);
            return;
        }

        generateSubsetSums(a, idx + 1, sum, res);
        generateSubsetSums(a, idx + 1, sum + a.get(idx), res);
    }

    public static boolean hasSubsetWithSum(long k, List<Integer> a) {
        int n = a.size();
        List<Integer> h1 = new ArrayList<>(a.subList(0, n/2));
        List<Integer> h2 = new ArrayList<>(a.subList(n/2, n));

        List<Long> s1 = new ArrayList<>();
        List<Long> s2 = new ArrayList<>();
        generateSubsetSums(h1, 0, 0L, s1);
        generateSubsetSums(h2, 0, 0L, s2);

        Collections.sort(s2);
        for (long sum1 : s1) {
            long target = k - sum1;
            if (Collections.binarySearch(s2, target) >= 0) {
                return true;
            }
        }
        return false;
    }
}
```

=== "Python"

```python
from bisect import bisect_left

def generate_subset_sums(a, i, current_sum, res):
    if i == len(a):
        res.append(current_sum)
        return
     
    generate_subset_sums(a, i + 1, current_sum, res)
    generate_subset_sums(a, i + 1, current_sum + a[i], res)

def has_subset_with_sum(k, a):
    n = len(a)
    h1 = a[:n//2]
    h2 = a[n//2:]

    print(h1)
    print(h2)

    s1, s2 = [], []
    generate_subset_sums(h1, 0, 0, s1)
    generate_subset_sums(h2, 0, 0, s2)

    s2.sort()
    for sum1 in s1:
        target = k - sum1
        idx = bisect_left(s2, target)
        if idx < len(s2) and s2[idx] == target:
            return True
    return False
```
