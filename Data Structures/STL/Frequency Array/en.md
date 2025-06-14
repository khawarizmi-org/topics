
### Frequency Arrays

Frequency arrays, also known as frequency tables, are data structures used to count occurrences of elements in a dataset efficiently, especially when the elements have a limited range. This technique is particularly useful in scenarios like counting, sorting, and quick lookups.

#### Basic Concept

A frequency array is an array where each index corresponds to a value in the dataset, and the value at that index represents the number of times that value appears.

For example, given an array of integers:

```
arr = [1, 3, 2, 3, 1, 5, 2, 2]
```

The frequency array would look like this:

```
frequency = [0, 2, 3, 2, 0, 1]
// Index 0: value 0 occurs 0 times
// Index 1: value 1 occurs 2 times
// Index 2: value 2 occurs 3 times
// Index 3: value 3 occurs 2 times
// Index 4: value 4 occurs 0 times
// Index 5: value 5 occurs 1 time
```

#### Advantages
- **Efficiency:** Frequency arrays can efficiently answer queries about the occurrence of specific elements.
- **Simplicity:** Easy implementation and intuitive representation.
- **Optimized Counting:** Enables O(1) complexity for frequency queries.

#### Use Cases
- **Sorting:** Frequency arrays facilitate counting sort, a non-comparison-based sorting algorithm.
- **Histogram Generation:** Quickly generate histograms for statistical analysis.
- **Pattern Recognition:** Useful in algorithms that require counting occurrences, like character frequency analysis in strings.

#### Limitations
- **Limited Range:** Ideal for datasets with a known and limited range of elements. Inefficient for large ranges or sparse datasets.
- **Memory Consumption:** Can be inefficient in terms of space if the range of values is significantly larger than the number of elements.

#### Example Implementation

=== c++

```c++
#include <iostream>
using namespace std;

int main() {
    int n;
    cin >> n;
    int arr[n];
    int max_element = 0; // we use this to know the size of the frequency array.

    for (int i = 0 ; i < n ; i++) {
        cin >> arr[i];
        max_element = max(max_element, arr[i]);
    }

    // the frequency array size should be max_element + 1,
    // because the array is 0-indexed.
    int freq[max_element + 1] = {}; // we make it equals to {} to make sure that all of it's elements are zeros at the begining.
    for (int i = 0 ; i < n ; i++) {
        freq[arr[i]]++; // each time we visit a number in the array, we increase its frequency by 1
    }

    for (int i = 1 ; i <= max_element ; i++) {
        cout << "The frequency of " << i << " is " << freq[i] << endl;
    }
}
```

=== Java

```java
import java.util.Scanner;

public class FrequencyArray {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int n;  
        n = sc.nextInt();
        int[] arr = new int[n];
        int max_element = 0; // we use this to know the size of the frequency array.

        for (int i = 0; i < n; i++) {
            arr[i] = sc.nextInt();
            max_element = Math.max(max_element, arr[i]);
        }

        // the frequency array size should be max_element + 1,
        // because the array is 0-indexed.
        int[] freq = new int[max_element + 1]; // we make it equals to {} to make sure that all of it's elements are zeros at the begining.
        for (int i = 0; i < n; i++) {
            freq[arr[i]]++; // each time we visit a number in the array, we increase its frequency by 1
        }

        for (int i = 1; i <= max_element; i++) {
            System.out.println("The frequency of " + i + " is " + freq[i]);
        }
    }
}
```

=== Python

```python
n = int(input())
arr = [0] * n
max_element = 0  # we use this to know the size of the frequency array.

for i in range(n):
    arr[i] = int(input())
    max_element = max(max_element, arr[i])

# the frequency array size should be max_element + 1,
# because the array is 0-indexed.
freq = [0] * (max_element + 1)  # we make it equals to {} to make sure that all of it's elements are zeros at the begining.
for i in range(n):
    freq[arr[i]] += 1  # each time we visit a number in the array, we increase its frequency by 1

for i in range(1, max_element + 1):
    print(f"The frequency of {i} is {freq[i]}")
```
