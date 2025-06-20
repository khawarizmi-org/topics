# Binary Search

Binary search is an efficient algorithm for finding a target value in a sorted array by repeatedly dividing the search interval in half. It runs in logarithmic time, $O(log n)$, by comparing the target to the middle element and narrowing the search range accordingly.

## Searching in sorted collections
The most common application/example of binary search can be found in the following problem:

Given a _**sorted**_ array of size `n`, where $A_0 \leq A_1 \leq A_2 ... \leq An$, find if a target value `target` is present in the array.

The simplest solution would be to check each element and return true if any element matches `target`. This is called linear search, and it is linear in the sense that it runs in linear time $O(n)$.

However, we can utilize the fact that the array is sorted in order to optimize the runtime of this search.

Take the array in the image below as an example. Let's say we want to find if the value `target = 7` exists in the array. 

Initially, our search range will be the entire array, and the algorithm states that we should start by checking the middle value in that search range (i.e. $M = n/2$).

* Initially, $A_M = 14$, $target = 7$
* Since $target < A_M$, we know that $target$ will be in the left-half of the array.
* The second value of $A_M$ would be $A_M = 6$
* Since $target > A_M$ in this case, we move to the right-side. But we only look for the portion that we did not exclude.
* Now, $A_M = 8$
* $target < A_M$, move to the left again.
* $A_M = target = 7$, target found.

![Image by https://commons.wikimedia.org/wiki/User:AlwaysAngry](images/Binary_Search_Depiction.svg)

### Why the middle value?
In the above example, we kept looking at the value in the middle of the search range. But is that the most optimal value to look for? Let's see this proof:

Let's define our search range in the array $A$ to be $[L,R]$. Assume that `target`, the value we're looking for, exists between indices $L$ and $R$; That is, $A_L \leq target \leq A_R$. 

To start the search, let's say we pick an arbitrary value $M$ such that $L \le M \le R$ and we check if $target \leq A_M$ or $target \geq A_M$. We will have 2 possible scenarios:

1. $A_L \leq target \leq A_M$. In this case, we reduce the search range from $[L, R]$ to $[L, M]$;
2. $A_M \leq target \leq A_R$. In this case, we reduce the search range from $[L, R]$ to $[M, R]$.

For an efficient processing, we would want to choose $M$ in a way that reduces the search range to a single element as quickly as possible.

In the worst case, 

## Implementation
==="c++"
```cpp
// Iterative binary search
int binary_search(vector<int> arr, int target) {
  int left = 0, right = arr.size() - 1;
  while (left <= right) {
    int mid = (left + right) / 2;
    if (target == arr[mid]) {
      return mid;
    } else if (target < arr[mid]) {
      right = mid - 1;
    } else {
      left = mid + 1;
    }
  }
  return -1;
}
```
```cpp
// Recursive binary search
int binary_search(const vector<int>& arr, int left, int right, int target) {
    if (left > right) return -1;
    int mid = left + (right - left) / 2;
    if (arr[mid] == target) return mid;
    if (arr[mid] > target)
        return binary_search(arr, left, mid - 1, target);
    return binary_search(arr, mid + 1, right, target);
}
```

## Applications of Binary Search
### Lower and upper bounds
### Local minima/maxima of a function
### Binary search on the answer