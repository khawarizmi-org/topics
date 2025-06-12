# Trie (Prefix Tree)

A **Trie** (also known as a **Prefix Tree**) is a data structure used to efficiently store and retrieve strings, especially when dealing with prefix‑related queries. Common applications include dictionaries, autocomplete, and spell‑checking.

---

## Key Concept

* Each node usually represents **one character**.  
* The path from the root to a node spells a prefix; if that node is marked *terminal* it represents a full word.  
* Nodes that share prefixes are stored **once**, saving memory and giving \(O(m)\) insert / search where \(m\) is the key’s length.

---

## Advantages

1. **Fast look‑ups** in \(O(m)\).  
2. **Prefix queries** are trivial (ideal for autocomplete).  
3. **Space sharing** for common prefixes.

---

## Core Operations

| Operation   | Description                                        |
|-------------|----------------------------------------------------|
| **insert**  | Add a word to the Trie.                            |
| **search**  | Check if a complete word exists.                   |
| **startsWith** | Does any word start with the given prefix?      |
| **delete**  | (optional) Remove a word and prune unused nodes.   |

---

## Reference Implementations

Below you’ll find equivalent code in C++, Python, and JavaScript.  

=== "C++"
```cpp
#include <iostream>
#include <string>
using namespace std;

struct Node {
    Node* child[26]{};
    bool end = false;
};

class Trie {
    Node* root = new Node();
    static int id(char c){ return c - 'a'; }
public:
    void insert(const string& w){
        Node* cur = root;
        for(char ch: w){
            int i = id(ch);
            if(!cur->child[i]) cur->child[i] = new Node();
            cur = cur->child[i];
        }
        cur->end = true;
    }
    bool search(const string& w){
        Node* cur = root;
        for(char ch: w){
            int i = id(ch);
            if(!(cur = cur->child[i])) return false;
        }
        return cur->end;
    }
    bool startsWith(const string& p){
        Node* cur = root;
        for(char ch: p){
            int i = id(ch);
            if(!(cur = cur->child[i])) return false;
        }
        return true;
    }
};
```

=== "Python"
```python
class TrieNode:
    __slots__ = ("child", "end")
    def __init__(self):
        self.child = {}
        self.end = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word: str):
        node = self.root
        for ch in word:
            node = node.child.setdefault(ch, TrieNode())
        node.end = True

    def _walk(self, s: str):
        node = self.root
        for ch in s:
            node = node.child.get(ch)
            if node is None:
                return None
        return node

    def search(self, word: str) -> bool:
        node = self._walk(word)
        return bool(node and node.end)

    def starts_with(self, prefix: str) -> bool:
        return self._walk(prefix) is not None
```

=== "JavaScript"
```js
class TrieNode{
  constructor(){
    this.child = new Map(); // char -> TrieNode
    this.end   = false;
  }
}
export class Trie{
  #root = new TrieNode();
  insert(word){
    let node = this.#root;
    for(const ch of word){
      if(!node.child.has(ch)) node.child.set(ch, new TrieNode());
      node = node.child.get(ch);
    }
    node.end = true;
  }
  #walk(str){
    let node = this.#root;
    for(const ch of str){
      node = node.child.get(ch);
      if(!node) return null;
    }
    return node;
  }
  search(word){ const n = this.#walk(word); return !!n && n.end; }
  startsWith(prefix){ return !!this.#walk(prefix); }
}
```
