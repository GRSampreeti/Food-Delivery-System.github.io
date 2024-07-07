# Radix Tree 

A radix tree (also known as radix trie, compact prefix tree, or compressed trie) is a data structure that represents a space-optimized trie (prefix tree) in which each node that is the only child is merged with its parent. The key advantages of radix trees include reduced space usage and improved efficiency for small sets and strings with long shared prefixes.

## Code Implementation

```cpp
#include <iostream>
#include <unordered_map>
#include <vector>
#include <string>
#include <tuple>

class RadixNode {
public:
    std::unordered_map<char, RadixNode*> nodes;
    bool is_leaf;
    std::string prefix;

    RadixNode(const std::string& prefix = "", bool is_leaf = false)
        : prefix(prefix), is_leaf(is_leaf) {}

    std::tuple<std::string, std::string, std::string> match(const std::string& word) {
        size_t x = 0;
        for (size_t i = 0; i < std::min(prefix.size(), word.size()); ++i) {
            if (prefix[i] != word[i]) break;
            ++x;
        }
        return { prefix.substr(0, x), prefix.substr(x), word.substr(x) };
    }

    void insert_many(const std::vector<std::string>& words) {
        for (const auto& word : words) {
            insert(word);
        }
    }

    void insert(const std::string& word) {
        if (prefix == word && !is_leaf) {
            is_leaf = true;
            return;
        }
        if (nodes.find(word[0]) == nodes.end()) {
            nodes[word[0]] = new RadixNode(word, true);
        } else {
            auto incoming_node = nodes[word[0]];
            std::string matching_string, remaining_prefix, remaining_word;
            std::tie(matching_string, remaining_prefix, remaining_word) = incoming_node->match(word);

            if (remaining_prefix.empty()) {
                incoming_node->insert(remaining_word);
            } else {
                incoming_node->prefix = remaining_prefix;
                auto aux_node = nodes[matching_string[0]];
                nodes[matching_string[0]] = new RadixNode(matching_string, false);
                nodes[matching_string[0]]->nodes[remaining_prefix[0]] = aux_node;

                if (remaining_word.empty()) {
                    nodes[matching_string[0]]->is_leaf = true;
                } else {
                    nodes[matching_string[0]]->insert(remaining_word);
                }
            }
        }
    }

    bool find(const std::string& word) {
        auto it = nodes.find(word[0]);
        if (it == nodes.end()) return false;
        auto incoming_node = it->second;
        std::string matching_string, remaining_prefix, remaining_word;
        std::tie(matching_string, remaining_prefix, remaining_word) = incoming_node->match(word);

        if (!remaining_prefix.empty()) return false;
        if (remaining_word.empty()) return incoming_node->is_leaf;
        return incoming_node->find(remaining_word);
    }

    bool delete_word(const std::string& word) {
        auto it = nodes.find(word[0]);
        if (it == nodes.end()) return false;
        auto incoming_node = it->second;
        std::string matching_string, remaining_prefix, remaining_word;
        std::tie(matching_string, remaining_prefix, remaining_word) = incoming_node->match(word);

        if (!remaining_prefix.empty()) return false;
        if (!remaining_word.empty()) return incoming_node->delete_word(remaining_word);
        if (!incoming_node->is_leaf) return false;

        if (incoming_node->nodes.empty()) {
            delete nodes[word[0]];
            nodes.erase(word[0]);
            if (nodes.size() == 1 && !is_leaf) {
                auto merging_node = nodes.begin()->second;
                is_leaf = merging_node->is_leaf;
                prefix += merging_node->prefix;
                nodes = merging_node->nodes;
                delete merging_node;
            }
        } else if (incoming_node->nodes.size() > 1) {
            incoming_node->is_leaf = false;
        } else {
            auto merging_node = incoming_node->nodes.begin()->second;
            incoming_node->is_leaf = merging_node->is_leaf;
            incoming_node->prefix += merging_node->prefix;
            incoming_node->nodes = merging_node->nodes;
            delete merging_node;
        }
        return true;
    }
};

void test_trie() {
    std::vector<std::string> words = { "banana", "bananas", "bandana", "band", "apple", "all", "beast" };
    RadixNode root;
    root.insert_many(words);

    for (const auto& word : words) {
        if (!root.find(word)) {
            std::cout << "Test failed: word not found - " << word << std::endl;
            return;
        }
    }

    if (root.find("bandanas") || root.find("apps")) {
        std::cout << "Test failed: unexpected word found." << std::endl;
        return;
    }

    root.delete_word("all");
    if (root.find("all")) {
        std::cout << "Test failed: word found after deletion - all" << std::endl;
        return;
    }

    root.delete_word("banana");
    if (root.find("banana")) {
        std::cout << "Test failed: word found after deletion - banana" << std::endl;
        return;
    }

    if (!root.find("bananas")) {
        std::cout << "Test failed: word not found - bananas" << std::endl;
        return;
    }

    std::cout << "All tests passed." << std::endl;
}

int main() {
    test_trie();
    return 0;
}

