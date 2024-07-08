# Merge Sort

Merge sort is a divide-and-conquer algorithm that divides the input array into two halves, recursively sorts each half, and then merges the sorted halves to produce the sorted array. This process continues until each sublist contains a single element, which is considered sorted. The merging step involves comparing elements from each sublist and combining them into a larger sorted list.

![Merge Sort Animation](https://upload.wikimedia.org/wikipedia/commons/c/cc/Merge-sort-example-300px.gif)
<p><em>Source: https://en.wikipedia.org/wiki/Merge_sort </em></p>

### Performance
- **Worst-case performance**: $O(n \log n)$
- **Best-case performance**: $\Omega(n \log n)$ (typical), $\Omega(n)$ (natural variant)
- **Average performance**: $\Theta(n \log n)$
- **Worst-case space complexity**: 
  - $O(n)$ total with $O(n)$ auxiliary
  - $O(1)$ auxiliary with linked lists


## Implementation in C

```c
#include <stdio.h>

void read(int a[], int n);
void display(int a[], int n);
void merge_sort(int a[], int l, int r);
void merge(int a[], int l, int m, int r);

int main() {
    int n, a[10000];
    printf("Enter the number of array elements: ");
    scanf("%d", &n);
    read(a, n);
    merge_sort(a, 0, n - 1);
    display(a, n);
    return 0;
}

void read(int a[], int n) {
    printf("Enter the array elements:\n");
    for(int i = 0; i < n; i++) {
        scanf("%d", &a[i]);
    }
}

void display(int a[], int n) {
    printf("Sorted array: ");
    for(int i = 0; i < n; i++) {
        printf("%d ", a[i]);
    }
    printf("\n");
}

void merge_sort(int a[], int l, int r) {
    if(l < r) {
        int m = l + (r - l) / 2;
        merge_sort(a, l, m);
        merge_sort(a, m + 1, r);
        merge(a, l, m, r);
    }
}

void merge(int a[], int l, int m, int r) {
    int i, j, k;
    int n1 = m - l + 1;
    int n2 = r - m;

    int L[n1], R[n2];
    for(i = 0; i < n1; i++) L[i] = a[l + i];
    for(j = 0; j < n2; j++) R[j] = a[m + 1 + j];

    i = 0;
    j = 0;
    k = l;
    while(i < n1 && j < n2) {
        if(L[i] <= R[j]) {
            a[k] = L[i];
            i++;
        } else {
            a[k] = R[j];
            j++;
        }
        k++;
    }

    while(i < n1) {
        a[k] = L[i];
        i++;
        k++;
    }

    while(j < n2) {
        a[k] = R[j];
        j++;
        k++;
    }
}
```
