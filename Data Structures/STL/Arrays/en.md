# Arrays

## Introduction  
An **array** is a collection of elements identified by index. Arrays store elements in contiguous memory, supporting constant-time access and update. In many languages, arrays have fixed size; dynamic arrays (e.g., vectors, lists) can resize.

---

## Declaration & Initialization

=== "c++"  
```cpp
// Fixed-size array
int a[5];               // default-initialized (undefined values for built-ins)
// Initialize explicitly
int b[5] = {1, 2, 3, 4, 5};
// Dynamic array using std::vector
#include <vector>
std::vector<int> v(5);           // size 5, values initialized to 0
std::vector<int> u = {1,2,3,4,5};
```

=== "Java"  
```java
// Fixed-size array
int[] a = new int[5];          // defaults to 0
// Initialize with values
int[] b = {1, 2, 3, 4, 5};
// Dynamic array via ArrayList
import java.util.ArrayList;
ArrayList<Integer> v = new ArrayList<>();
v.add(1);
v.add(2);
// ...
```

=== "Python"  
```python
# List as dynamic array
a = [0] * 5                    # [0,0,0,0,0]
b = [1,2,3,4,5]
```

---

## Access & Update  
Arrays support $O(1)$ indexing.

=== "c++"
```cpp
a[2] = 10;          // update
int x = a[4];       // access
```

=== "Java"
```java
b[1] = 7;
int x = b[3];
```

=== "Python"
```python
u[0] = 42
x = u[2]
```

---

## Traversal  
Iterate over all elements ($O(n)$).

=== "c++"  
```cpp
for (int i = 0; i < n; ++i) {
    cout << a[i] << " ";
}
```

=== "Java"  
```java
for (int i = 0; i < a.length; i++) {
    System.out.print(a[i] + " ");
}
```

=== "Python"  
```python
for x in a:
    print(x, end=" ")
```

---

## Common Patterns

### Sliding Window  
Compute max sum of any subarray of size $k$:
```cpp
int maxSum(vector<int>& v, int k) {
    int n = v.size();
    int sum = 0;
    for (int i = 0; i < k; ++i) sum += v[i];
    int ans = sum;
    for (int i = k; i < n; ++i) {
        sum += v[i] - v[i-k];
        ans = max(ans, sum);
    }
    return ans;
}
```

```java
int maxSum(int[] arr, int k) {
    int sum = 0, ans = 0;
    for (int i = 0; i < k; i++) sum += arr[i];
    ans = sum;
    for (int i = k; i < arr.length; i++) {
        sum += arr[i] - arr[i-k];
        ans = Math.max(ans, sum);
    }
    return ans;
}
```

```python
def max_sum(arr, k):
    s = sum(arr[:k])
    ans = s
    for i in range(k, len(arr)):
        s += arr[i] - arr[i-k]
        ans = max(ans, s)
    return ans
```

### Two Pointers  
Check if array has two elements summing to target (sorted):
```cpp
bool twoSum(vector<int>& v, int target) {
    int l = 0, r = v.size()-1;
    while (l < r) {
        int s = v[l] + v[r];
        if (s == target) return true;
        else if (s < target) ++l;
        else --r;
    }
    return false;
}
```

```java
boolean twoSum(int[] a, int target) {
    int l = 0, r = a.length-1;
    while (l < r) {
        int s = a[l] + a[r];
        if (s == target) return true;
        if (s < target) l++;
        else r--;
    }
    return false;
}
```

```python
def two_sum(a, target):
    l, r = 0, len(a)-1
    while l < r:
        s = a[l] + a[r]
        if s == target: return True
        if s < target: l += 1
        else: r -= 1
    return False
```

---

## Multidimensional Arrays

=== "c++"
```cpp
int mat[3][4];             // 3 rows, 4 columns
vector<vector<int>> M(3, vector<int>(4, 0));
```

=== "Java"
```java
int[][] mat = new int[3][4];   // all zeros
```

=== "Python"
```python
M = [[0]*4 for _ in range(3)]
```

---

## Summary
- Arrays allow fast $O(1)$ access and update by index.
- Traversal, sorting, and search patterns often involve $O(n)$ or $O(n \log n)$.
- Know common patterns: sliding window, two pointers.
- Use language-specific dynamic arrays when size changes at runtime.
