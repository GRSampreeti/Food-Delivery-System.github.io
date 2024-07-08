# Fisher-Yates Shuffle Algorithm

The Fisher-Yates shuffle algorithm is used to generate a random permutation of a finite sequence. The algorithm starts from the last element and swaps it with a randomly selected element from the whole array (including itself). It then proceeds to the second last element, and so on, until it reaches the first element.

The time complexity of this algorithm is \( O(n) \).

<img src="https://upload.wikimedia.org/wikipedia/commons/5/5b/Durstenfeld_shuffle.svg" alt="Fisher-Yates Shuffle" width="300">

## C++ Implementation

```cpp
#include <bits/stdc++.h>
#include <stdlib.h>
#include <time.h>
using namespace std;

// A utility function to swap two integers
void swap(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

// A utility function to print an array
void printArray(int arr[], int n) {
    for (int i = 0; i < n; i++)
        cout << arr[i] << " ";
    cout << "\n";
}

// A function to generate a random permutation of arr[]
void randomize(int arr[], int n) {
    // Use a different seed value so that
    // we don't get same result each time
    // we run this program
    srand(time(NULL));

    // Start from the last element and swap
    // one by one. We don't need to run for
    // the first element that's why i > 0
    for (int i = n - 1; i > 0; i--) {
        // Pick a random index from 0 to i
        int j = rand() % (i + 1);

        // Swap arr[i] with the element
        // at random index
        swap(&arr[i], &arr[j]);
    }
}

// Driver Code
int main() {
    int arr[] = {1, 2, 3, 4, 5, 6, 7, 8};
    int n = sizeof(arr) / sizeof(arr[0]);
    randomize(arr, n);
    printArray(arr, n);

    return 0;
}
```
