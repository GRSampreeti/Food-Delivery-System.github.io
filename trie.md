# Trie Data Structure

Trie (pronounced "try") is a tree-like data structure used for efficient retrieval of key-value pairs where keys are usually strings. It is particularly useful for scenarios like predictive text, spell checking, and approximate string matching.

## Overview

Tries store strings in a tree-like structure where each node represents a letter of the alphabet. Nodes may have links to other nodes, representing subsequent letters in the string. Here's a breakdown of operations and complexities associated with Tries:

### Operations

- **Insertion:** Insert a word into the Trie.
- **Search:** Check if a word exists in the Trie.
- **StartsWith:** Check if there are any words in the Trie that start with a given prefix.

### Time Complexity

| Operation   | Average Case | Worst Case |
|-------------|--------------|------------|
| Search      | O(len)       | O(len)     |
| Insertion   | O(len)       | O(len)     |
| StartsWith  | O(len)       | O(len)     |

Here, `len` denotes the length of the word or prefix being processed.

### Space Complexity

- Space: O(n), where n is the total number of characters stored in the Trie.

## Implementation

```cpp
#include <iostream>
using namespace std;

// Node structure for Trie
struct Node {
    Node* links[26]; // Array to store links to child nodes (for each letter 'a' to 'z')
    bool flag = false; // Flag indicating if the node marks the end of a word

    bool containsKey(char ch) {
        return links[ch - 'a'] != NULL;
    }

    void put(char ch, Node* node) {
        links[ch - 'a'] = node;
    }

    Node* get(char ch) {
        return links[ch - 'a'];
    }

    void setEnd() {
        flag = true;
    }

    bool isEnd() {
        return flag;
    }
};

// Trie class
class Trie {
private:
    Node* root;

public:
    Trie() {
        root = new Node();
    }

    void insert(string word) {
        Node* node = root;
        for (int i = 0; i < word.length(); i++) {
            if (!node->containsKey(word[i])) {
                node->put(word[i], new Node());
            }
            node = node->get(word[i]);
        }
        node->setEnd();
    }

    bool search(string word) {
        Node* node = root;
        for (int i = 0; i < word.length(); i++) {
            if (!node->containsKey(word[i])) {
                return false;
            }
            node = node->get(word[i]);
        }
        return node->isEnd();
    }

    bool startsWith(string prefix) {
        Node* node = root;
        for (int i = 0; i < prefix.length(); i++) {
            if (!node->containsKey(prefix[i])) {
                return false;
            }
            node = node->get(prefix[i]);
        }
        return true;
    }
};

int main() {
    Trie trie;

    // Inserting words into the Trie
    trie.insert("sampreeti");
    trie.insert("sammati");
    trie.insert("Abhijna");

    // Searching for words in the Trie
    cout << "Search for 'strawberry': " << (trie.search("strawberry") ? "True" : "False") << endl;
    cout << "Search for 'sampreeti': " << (trie.search("sampreeti") ? "True" : "False") << endl;

    return 0;
}
```
