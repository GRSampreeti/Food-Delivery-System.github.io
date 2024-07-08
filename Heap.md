## Heap

A heap is a tree-based data structure that satisfies the heap property:

- **Insertion:** Add the new element at the end of the heap, in the first available free space. If this violates the heap property, perform a "swim" operation (sift up) until the heap property is restored.
  
- **Extraction:** Remove the root element and replace it with the last element of the heap. If this violates the heap property, perform a "sink" operation (sift down) on the new root to restore the heap property.
  
- **Replacement:** Remove the root element, replace it with a new element, and perform a sift down operation if necessary. This method avoids a separate insertion step after extraction.


![Heap Sort Example](https://upload.wikimedia.org/wikipedia/commons/4/4d/Heapsort-example.gif)

### Code

```cpp
#include <stdio.h>

void Insert(int A[], int n) {
    int i = n, temp;
    temp = A[i];
    while (i > 1 && temp > A[i / 2]) {
        A[i] = A[i / 2];
        i = i / 2;
    }
    A[i] = temp;
}

int Delete(int A[], int n) {
    int i, j, x, temp, val;
    val = A[1];
    x = A[n];
    A[1] = A[n];
    A[n] = val;
    i = 1;
    j = i * 2;
    while (j <= n - 1) {
        if (j < n - 1 && A[j + 1] > A[j]) {
            j = j + 1;
        }
        if (A[i] < A[j]) {
            temp = A[i];
            A[i] = A[j];
            A[j] = temp;
            i = j;
            j = 2 * j;
        } else {
            break;
        }
    }
    return val;
}

int main() {
    int H[] = {0, 14, 15, 5, 20, 30, 8, 40};
    int i;
    
    for (i = 2; i <= 7; i++) {
        Insert(H, i);
    }
    
    for (i = 7; i > 1; i--) {
        Delete(H, i);
    }
    
    for (i = 1; i <= 7; i++) {
        printf("%d ", H[i]);
    }
    printf("\n");
    
    return 0;
}
```
