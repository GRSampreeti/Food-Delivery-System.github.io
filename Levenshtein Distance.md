# Levenshtein Distance Example

The Levenshtein distance measures the difference between two sequences by calculating the minimum number of single-character edits (insertions, deletions, substitutions) required to change one word into another. It is widely used in approximate string matching for applications like spell checkers, optical character recognition correction, and natural-language translation. The metric is also used in linguistics to quantify linguistic distance and mutual intelligibility between languages. While efficient for short strings, its computational cost makes it impractical for longer strings.

## Time Complexity
- **Time complexity**: O(3^(m+n))
- **Auxiliary complexity**: O(m+n)

## Code

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>

// Structure to hold item and its Levenshtein distance
struct ItemDistance {
    std::string item;
    int distance;

    bool operator<(const ItemDistance& other) const {
        return distance < other.distance;
    }
};

// Function to compute the Levenshtein distance between two strings
int getLevenshteinDistance(const std::string& s1, const std::string& s2) {
    int len1 = s1.size();
    int len2 = s2.size();
    std::vector<std::vector<int>> dp(len1 + 1, std::vector<int>(len2 + 1));

    for (int i = 0; i <= len1; ++i) {
        for (int j = 0; j <= len2; ++j) {
            if (i == 0) {
                dp[i][j] = j;
            } else if (j == 0) {
                dp[i][j] = i;
            } else if (s1[i - 1] == s2[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1];
            } else {
                dp[i][j] = 1 + std::min({dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1]});
            }
        }
    }
    return dp[len1][len2];
}

// Function to find related items based on Levenshtein distance
void findRelatedItems(const std::string& searchTerm, const std::vector<std::string>& items) {
    std::vector<ItemDistance> distances;

    for (const auto& item : items) {
        int distance = getLevenshteinDistance(searchTerm, item);
        distances.push_back({item, distance});
    }

    std::sort(distances.begin(), distances.end());

    std::cout << "Related Searches:\n";
    for (int i = 0; i < std::min(5, static_cast<int>(distances.size())); ++i) {
        std::cout << distances[i].item << " (" << distances[i].distance << ")\n";
    }
}

int main() {
    std::vector<std::string> items = {
        "Pizza", "Burger", "Sushi", "Pasta", "Salad",
        "Tacos", "Ramen", "Steak", "Sandwich", "Curry",
        "Burrito", "Fries", "Soup", "Noodles", "Wings",
        "DimSum", "BBQ", "Pho", "Kebab", "IceCream"
    };

    while (true) {
        int choice;
        std::cout << "1 - Search\n0 - Exit\n";
        std::cin >> choice;

        if (choice == 1) {
            std::string searchTerm;
            std::cout << "Enter search term: ";
            std::cin.ignore();
            std::getline(std::cin, searchTerm);
            findRelatedItems(searchTerm, items);
        } else if (choice == 0) {
            break;
        }
    }

    return 0;
}
```
