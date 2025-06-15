# Map

#### Map (`map`/`TreeMap`)

In CP, you often need to count or look up items whose keys aren’t small integers (e.g. strings, pairs, sparse IDs). A frequency-array fails in those cases—use associative containers instead.

---

### Description

- **`map<K,V>` (C++) / `TreeMap<K,V>` (Java)**  
  - Balanced BST under the hood (usually a Red–Black tree).  
  - Keys stored in sorted order.  

---

### When to Use

| Scenario                                    | Frequency Array | `map` / `TreeMap`        |
|---------------------------------------------|:---------------:|:------------------------:|
| Keys are **contiguous small** integers      | ✓               | ✓ (but slower)           |
| Keys are **large/sparse** integers (e.g. IDs up to 10⁹) | ✗  | ✓                        |
| Keys are **strings** (words, substrings)    | ✗               | ✓                        |
| Keys are **pairs/tuples** (coordinates)     | ✗               | ✓                        |
| Need **ordered iteration** or range queries | ✗               | ✓                        |

---

### Time Complexity

| Operation      | `map` / `TreeMap` |
|:--------------:|:-----------------:|
| **Insert**     | O(log n)          |
| **Find/Access**| O(log n)          |
| **Erase**      | O(log n)          |
| **Iterate**    | O(n) in sorted order |

---

### Implementation Examples

#### Example for insertion and iteration

=== c++

```cpp
#include <iostream>
#include <map>
#include <string>
using namespace std;

int main(){
    map<string, int> frequency;

    string arr[] = {
        "cat", "hat", "apple",
        "apple", "hat", "dog",
        "hat", "cat", "cat",
        "cat", "hat", "hat"
    };

    for (string &s : arr){
        // increment the frequency of each string
        // no need to check if the key exists, map will handle it
        frequency[s]++; 
    }

    cout << "Map keys and values:" << endl;
    for (auto [key, value] : frequency) {
        cout << key << " -> " << value << endl;
    }

    return 0;
}
```

=== Java

```java
import java.util.Map;
import java.util.TreeMap;

public class FrequencyCounter {
    public static void main(String[] args) {
        Map<String, Integer> frequency = new TreeMap<>();

        String[] arr = {
            "cat", "hat", "apple",
            "apple", "hat", "dog",
            "hat", "cat", "cat",
            "cat", "hat", "hat"
        };

        for (String s : arr) {
            // increment the frequency of each string;
            // no need to check if the key exists, getOrDefault handles it
            frequency.put(s, frequency.getOrDefault(s, 0) + 1);
        }

        System.out.println("Map keys and values:");
        for (Map.Entry<String, Integer> entry : frequency.entrySet()) {
            System.out.println(entry.getKey() + " -> " + entry.getValue());
        }
    }
}
```


The output will be:

```plaintext
Map keys and values:
apple -> 2
cat -> 4
dog -> 1
hat -> 5
```

As you can see, the keys are stored in sorted order, and the frequency of each string is counted correctly.

#### Example for checking the frequency of a specific key and deleting it

=== c++

```cpp
#include <iostream>
#include <map>
#include <string>
using namespace std;

int main(){
    map<string, int> frequency;

    string arr[] = {
        "cat", "hat", "apple",
        "apple", "hat", "dog",
        "hat", "cat", "cat",
        "cat", "hat", "hat"
    };

    for (string &s : arr){
        // increment the frequency of each string
        // no need to check if the key exists, map will handle it
        frequency[s]++; 
    }

    // To access if a key exists and its value
    // we can treat the map like an array
    cout << "Frequency of cat: " << frequency["cat"] << endl;
    cout << endl;

    // And if we want to check if the key exists, we can use `count`
    cout << "Does banana exist? " << frequency.count("banana") << endl;
    cout << "Does apple exist? " << frequency.count("apple") << endl;
    cout << endl;

    // We can also delete a key from the map
    cout << "number of elements in the map is: " << frequency.size() << endl;
    frequency.erase("cat");
    cout << "number of elements in the map is: " << frequency.size() << endl;

    return 0;
}
```

=== Java

```java
import java.util.Map;
import java.util.TreeMap;

public class FrequencyMapDemo {
    public static void main(String[] args) {
        Map<String, Integer> frequency = new TreeMap<>();

        String[] arr = {
            "cat", "hat", "apple",
            "apple", "hat", "dog",
            "hat", "cat", "cat",
            "cat", "hat", "hat"
        };

        // Build the frequency map
        for (String s : arr) {
            frequency.put(s, frequency.getOrDefault(s, 0) + 1);
        }

        // Accessing a key's value directly
        System.out.println("Frequency of cat: " + frequency.get("cat"));
        System.out.println();

        // Checking if a key exists (0 if absent, 1 if present)
        System.out.println("Does banana exist? " + (frequency.containsKey("banana") ? 1 : 0));
        System.out.println("Does apple exist?  " + (frequency.containsKey("apple")  ? 1 : 0));
        System.out.println();

        // Map size before and after removal
        System.out.println("Number of elements in the map is: " + frequency.size());
        frequency.remove("cat");
        System.out.println("Number of elements in the map is: " + frequency.size());
    }
}
```

The output will be:

```plaintext
Frequency of cat: 4

Does banana exist? 0
Does apple exist?  1

Number of elements in the map is: 4
Number of elements in the map is: 3
```

---

### Why Maps Over Frequency Arrays?

1. **Arbitrary Key Types**:  
   - **Strings**: word/sub-string frequencies.  
   - **Pairs/Tuples**: coordinate or edge counts.  

2. **No Predefined Range**:  
   - Frequency array requires knowing `max_key` and allocating `O(max_key)`.  
   - Maps grow dynamically in `O(log n)`

3. **Sparse Data**:  
   - If only a few keys out of a huge range appear, arrays waste memory.

4. **Ordered Operations** (with `map`/`TreeMap`):  
   - Range queries: `lower_bound`, `upper_bound`.  
   - In-order traversal.
