# Bloom Filter

A Bloom filter is a space-efficient probabilistic data structure that is used to test whether an item is a member of a set. The Bloom filter will always say yes if an item is a set member. However, the Bloom filter might still say yes although an item is not a member of the set (false positive). The items can be added to the Bloom filter but the items cannot be removed. The Bloom filter supports the following operations:

- Adding an item to the set
- Test the membership of an item in the set

![Adding an item to Bloom Filter](https://systemdesign.one/bloom-filters-explained/add-item-bloom-filter.webp)
![Testing membership in Bloom Filter](https://systemdesign.one/bloom-filters-explained/item-membership-bloom-filter.webp)

## C++ Code

```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;

// hash 1
int h1(string s, int arrSize) {
    ll int hash = 0;
    for (int i = 0; i < s.size(); i++) {
        hash = (hash + ((int)s[i])) % arrSize;
    }
    return hash;
}

// hash 2
int h2(string s, int arrSize) {
    ll int hash = 1;
    for (int i = 0; i < s.size(); i++) {
        hash = (hash + pow(19, i) * s[i]) % arrSize;
    }
    return hash % arrSize;
}

// hash 3
int h3(string s, int arrSize) {
    ll int hash = 7;
    for (int i = 0; i < s.size(); i++) {
        hash = (hash * 31 + s[i]) % arrSize;
    }
    return hash % arrSize;
}

// hash 4
int h4(string s, int arrSize) {
    ll int hash = 3;
    int p = 7;
    for (int i = 0; i < s.size(); i++) {
        hash = (hash * 7 + s[i] * pow(p, i)) % arrSize;
    }
    return hash;
}

// lookup operation
bool lookup(bool* bitarray, int arrSize, string s) {
    int a = h1(s, arrSize);
    int b = h2(s, arrSize);
    int c = h3(s, arrSize);
    int d = h4(s, arrSize);

    if (bitarray[a] && bitarray[b] && bitarray[c] && bitarray[d])
        return true;
    else
        return false;
}

// insert operation
void insert(bool* bitarray, int arrSize, string s) {
    // check if the element in already present or not
    if (lookup(bitarray, arrSize, s))
        cout << s << " is probably already present" << endl;
    else {
        int a = h1(s, arrSize);
        int b = h2(s, arrSize);
        int c = h3(s, arrSize);
        int d = h4(s, arrSize);

        bitarray[a] = true;
        bitarray[b] = true;
        bitarray[c] = true;
        bitarray[d] = true;

        cout << s << " inserted" << endl;
    }
}

// Driver Code
int main() {
    bool bitarray[100] = { false };
    int arrSize = 100;
    string restaurantNames[] = { "PizzaHut", "Dominos", "Subway", "Starbucks", "KFC", "BurgerKing", "TacoBell", "McDonalds", "Chipotle", "Wendys" };

    for (int i = 0; i < 10; i++) {
        insert(bitarray, arrSize, restaurantNames[i]);
    }

    // Test the membership
    string testNames[] = { "PizzaHut", "PandaExpress", "KFC", "OliveGarden", "BurgerKing" };
    for (int i = 0; i < 5; i++) {
        if (lookup(bitarray, arrSize, testNames[i]))
            cout << testNames[i] << " is probably in the set." << endl;
        else
            cout << testNames[i] << " is not in the set." << endl;
    }

    return 0;
}
