# Trie (Prefix Tree)

A **Trie** (also known as a **Prefix Tree**) is a data structure used to efficiently store and retrieve strings, especially when dealing with prefix-related queries. It is commonly employed in applications such as dictionary implementations, autocomplete functionality, spell checking, and more.

## Key Concept
- A Trie is composed of connected **nodes** where each node typically represents a single character of a string.
- The path from the root node to a leaf node corresponds to the sequence of characters in a word.
- Each node may contain a marker indicating whether it represents the end of a valid word.

By sharing common paths between strings that share prefixes, the Trie helps save memory and provides efficient insert and search operations.

---

## Advantages
1. **Search Speed**: You can check if a word exists in \(O(m)\) time, where \(m\) is the length of the word.
2. **Shared Paths**: A Trie stores only one copy of a shared prefix for multiple words, saving space.
3. **Prefix Matching**: It is ideal for quickly finding all words that start with a certain prefix (e.g., autocomplete).

---

## Common Operations

Tries typically provide the following operations:

1. **Insert**: Insert a word into the Trie.
2. **Search**: Check if a word exists in the Trie.
3. **StartsWith** (optional): Check if any word in the Trie starts with a given prefix.
4. **Delete** (optional): Remove a word from the Trie (requires special handling to clean up unused nodes).

Below is a complete example in **C++** demonstrating these basic operations:

```cpp
#include <iostream>
#include <string>
using namespace std;

// A Trie node
struct TrieNode {
    // For English letters a-z, we use an array of size 26
    TrieNode* children[26];
    // Indicates if this node represents the end of a word
    bool isEndOfWord;

    TrieNode() {
        for (int i = 0; i < 26; i++) {
            children[i] = nullptr;
        }
        isEndOfWord = false;
    }
};

// Trie class
class Trie {
private:
    TrieNode* root;

    // Helper function to convert a character to an index (0-25)
    int charToIndex(char c) {
        return c - 'a';
    }

public:
    Trie() {
        root = new TrieNode();
    }

    // Insert a word into the Trie
    void insert(const string &key) {
        TrieNode* currentNode = root;
        for (char c : key) {
            int index = charToIndex(c);
            // If the child does not exist, create a new node
            if (!currentNode->children[index]) {
                currentNode->children[index] = new TrieNode();
            }
            currentNode = currentNode->children[index];
        }
        // Mark the end of the word
        currentNode->isEndOfWord = true;
    }

    // Search for a word in the Trie
    bool search(const string &key) {
        TrieNode* currentNode = root;
        for (char c : key) {
            int index = charToIndex(c);
            // If the child is missing, the word does not exist
            if (!currentNode->children[index]) {
                return false;
            }
            currentNode = currentNode->children[index];
        }
        // Must be a valid end of a word
        return (currentNode != nullptr && currentNode->isEndOfWord);
    }

    // Check if there is any word in the Trie that starts with a given prefix
    bool startsWith(const string &prefix) {
        TrieNode* currentNode = root;
        for (char c : prefix) {
            int index = charToIndex(c);
            if (!currentNode->children[index]) {
                return false;
            }
            currentNode = currentNode->children[index];
        }
        return true; // We reached the end of the prefix without interruption
    }
};

int main() {
    Trie trie;

    trie.insert("hello");
    trie.insert("hell");
    trie.insert("help");
    trie.insert("helium");
    trie.insert("cat");

    cout << boolalpha; // Print boolean values as 'true' or 'false' instead of 1/0

    // Test searching for words
    cout << "Search for 'hello': " << trie.search("hello") << endl; // true
    cout << "Search for 'hell':  " << trie.search("hell") << endl;  // true
    cout << "Search for 'help':  " << trie.search("help") << endl;  // true
    cout << "Search for 'hel':   " << trie.search("hel") << endl;   // false

    // Test if prefixes exist
    cout << "Starts with 'he':   " << trie.startsWith("he") << endl; // true
    cout << "Starts with 'ca':   " << trie.startsWith("ca") << endl; // true
    cout << "Starts with 'ho':   " << trie.startsWith("ho") << endl; // false

    return 0;
}
