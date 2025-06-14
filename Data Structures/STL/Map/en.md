# Map

#### Map (`map`/`TreeMap`) and Hash-Map (`unordered_map`/`HashMap`/`dict`)

In CP, you often need to count or look up items whose keys aren’t small integers (e.g. strings, pairs, sparse IDs). A frequency-array fails in those cases—use associative containers instead.

---

### Description

- **`map<K,V>` (C++) / `TreeMap<K,V>` (Java)**  
  - Balanced BST under the hood (usually a Red–Black tree).  
  - Keys stored in sorted order.  
- **`unordered_map<K,V>` (C++) / `HashMap<K,V>` (Java) / `dict` (Python)**  
  - Hash table with amortized O(1) operations.  
  - No ordering guarantees.

---

### When to Use

| Scenario                                    | Frequency Array | `map` / `TreeMap`        | `unordered_map` / `HashMap` / `dict` |
|---------------------------------------------|:---------------:|:------------------------:|:------------------------------------:|
| Keys are **contiguous small** integers      | ✓               | ✓ (but slower)           | ✓ (but needs hash)                   |
| Keys are **large/sparse** integers (e.g. IDs up to 10⁹) | ✗  | ✓                        | ✓                                    |
| Keys are **strings** (words, substrings)    | ✗               | ✓                        | ✓                                    |
| Keys are **pairs/tuples** (coordinates)     | ✗               | ✓                        | ✓                                    |
| Need **ordered iteration** or range queries | ✗               | ✓                        | ✗                                    |

---

### Time Complexity

| Operation      | `map` / `TreeMap` | `unordered_map`/`HashMap`/`dict` |
|:--------------:|:-----------------:|:--------------------------------:|
| **Insert**     | O(log n)          | O(1) amortized, O(n) worst       |
| **Find/Access**| O(log n)          | O(1) amortized, O(n) worst       |
| **Erase**      | O(log n)          | O(1) amortized, O(n) worst       |
| **Iterate**    | O(n) in sorted order | O(n) in arbitrary order      |

---

### Implementation Examples

=== c++

```cpp
#include <iostream>
#include <map>
#include <string>
#include <unordered_map>
using namespace std;

int main(){
    // Count frequencies of arbitrary keys (e.g. strings)
    unordered_map<string, int> freq; // O(1) avg operations
    map<string, int> ofreq;          // O(log n) ops, sorted

    string arr[] = {
    "cat", "hat", "apple",
    "apple", "hat", "dog",
    "hat", "cat", "cat",
    "cat", "hat", "hat"};

    for (string &s : arr){
        freq[s]++;  // hash map
        ofreq[s]++; // ordered map
    }

    cout << "Map keys and values:\n"; // Example lookup and in-order iteration
    for (auto &p : ofreq){
        // p.first = key, p.second = count
        cout << p.first << " -> " << p.second << "\n";
    }
    cout << "----------------------------------------------------" << endl;
    cout << "Frequency of cat:    " << ofreq["cat"] << endl;    // prints 4
    cout << "Frequency of apple:  " << ofreq["apple"] << endl;  // prints 2
    cout << "Frequency of banana: " << ofreq["banana"] << endl; // prints 0, since it is a new key it inserts it to the map and assign it the value 0
    cout << "----------------------------------------------------" << endl;

    cout << "Map keys and values:\n"; // Example lookup and in-order iteration after the modification
    for (auto &p : ofreq){
        // p.first = key, p.second = count
        cout << p.first << " -> " << p.second << "\n";
    }

    cout << "----------------------------------------------------" << endl;
    cout << "Frequency of dog: " << ofreq["dog"] << endl; // prints 1
    ofreq.erase("dog");
    cout << "Frequency of dog: " << ofreq["dog"] << endl; // prints 0 since the key was erased from the map

    ofreq.erase("car"); // nothing happens if the key didn't exist
    return 0;
}

// output
/*
Map keys and values:
apple -> 2
cat -> 4
dog -> 1
hat -> 5
----------------------------------------------------
Frequency of cat:    4
Frequency of apple:  2
Frequency of banana: 0
----------------------------------------------------
Map keys and values:
apple -> 2
banana -> 0
cat -> 4
dog -> 1
hat -> 5
----------------------------------------------------
Frequency of dog: 1
Frequency of dog: 0
*/
```

=== Java

```java
import java.util.*;

public class MapDemo {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        HashMap<String,Integer> hmap = new HashMap<>();   // O(1) avg
        TreeMap<String,Integer> omap = new TreeMap<>();   // O(log n)

        while (n-- > 0) {
            String s = sc.next();
            hmap.put(s, hmap.getOrDefault(s, 0) + 1);
            omap.put(s, omap.getOrDefault(s, 0) + 1);
        }

        // Ordered iteration
        for (var e : omap.entrySet()) {
            System.out.println(e.getKey() + " -> " + e.getValue());
        }
        System.out.println("Lookup "foo": " + hmap.getOrDefault("foo", 0));
    }
}
```

=== Python

```python
from collections import defaultdict

n = int(input())
hmap = defaultdict(int)   # dict with default 0

for _ in range(n):
    s = input().strip()
    hmap[s] += 1          # O(1) avg

# Iterate in sorted order if needed:
for key in sorted(hmap):
    print(f"{key} -> {hmap[key]}")

print("Lookup "foo":", hmap["foo"])
```

---

### Why Maps Over Frequency Arrays?

1. **Arbitrary Key Types**:  
   - **Strings**: word/sub-string frequencies.  
   - **Pairs/Tuples**: coordinate or edge counts.  

2. **No Predefined Range**:  
   - Frequency array requires knowing `max_key` and allocating `O(max_key)`.  
   - Maps grow dynamically in `O(log n)` or `O(1)`.

3. **Sparse Data**:  
   - If only a few keys out of a huge range appear, arrays waste memory.

4. **Ordered Operations** (with `map`/`TreeMap`):  
   - Range queries: `lower_bound`, `upper_bound`.  
   - In-order traversal.

```cpp
// Example: counting edges in a graph
map<pair<int,int>,int> edgeCount;
edgeCount[{u,v}]++;      // no huge array needed
```

---

> **Bottom Line:**  
> - **Frequency arrays** are unbeatable for tiny, known integer ranges.  
> - **Maps / Hash maps** handle **strings**, **pairs**, **sparse** or **unknown** keys with minimal cost.  
