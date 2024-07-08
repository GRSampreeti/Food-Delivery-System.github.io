# Binary Indexed Tree (Fenwick Tree)

A Fenwick tree or binary indexed tree (BIT) is a data structure that can efficiently update values and calculate prefix sums in an array of values.

## Time Complexity in Big O Notation

| Operation | Average | Worst Case |
|-----------|---------|------------|
| Search    | \( O(log n) \) | \( O(log n) \) |
| Insert    | \( O(log n) \) | \( O(log n) \) |

## Space Complexity

| Space | Complexity |
|-------|------------|
| Space | \( O(n) \) |

## C++ Code

```cpp
#include <iostream>

using namespace std;

int getSum(int BITree[], int index) {
    int sum = 0;
    index = index + 1;
    for (; index > 0; index -= index & (-index)) {
        sum += BITree[index];
    }
    return sum;
}

void updateBIT(int BITree[], int n, int index, int val) {
    index = index + 1;
    for (; index <= n; index += index & (-index)) {
        BITree[index] += val;
    }
}

int* constructBITree(int arr[], int n) {
    int* BITree = new int[n + 1];
    for (int i = 1; i <= n; i++)
        BITree[i] = 0;
    for (int i = 0; i < n; i++)
        updateBIT(BITree, n, i, arr[i]);
    return BITree;
}

int main() {
    int freq[] = {2, 1, 1, 3, 2, 3, 4, 5, 6, 7, 8, 9};
    int n = sizeof(freq) / sizeof(freq[0]);
    int* BITree = constructBITree(freq, n);
    cout << "Sum of elements in arr[0..5] is " << getSum(BITree, 5);
    freq[3] += 6;
    updateBIT(BITree, n, 3, 6);
    cout << "\nSum of elements in arr[0..5] after update is " << getSum(BITree, 5);
    delete[] BITree;
    return 0;
}
```
