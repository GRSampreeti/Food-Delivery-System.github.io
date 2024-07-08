# Quicksort

Quicksort is an efficient, general-purpose sorting algorithm. Quicksort is a divide-and-conquer algorithm. It works by selecting a 'pivot' element from the array and partitioning the other elements into two sub-arrays, according to whether they are less than or greater than the pivot.

![Quicksort Animation](https://upload.wikimedia.org/wikipedia/commons/6/6a/Sorting_quicksort_anim.gif)

## Performance Characteristics

| Performance | Complexity |
|-------------|-------------|
| Worst-case performance | \( O(n^2) \) |
| Best-case performance | \( O(n log n) \) (simple partition) or \( O(n) \) (three-way partition and equal keys) |
| Average performance | \( O(n log n) \) |
| Worst-case space complexity | \( O(n) \) auxiliary (naive) or \( O(log n) \) auxiliary |

## C Code

```c
#include<stdio.h>

void read(int a[], int n) {
    int i;
    for(i = 0; i < n; i++) {
        scanf("%d", &a[i]);
    }
}

void display(int a[], int n) {
    int i;
    for(i = 0; i < n; i++) {
        printf("%d ", a[i]);
    }
    printf("\n");
}

void quick(int a[], int l, int h) {
    int i, j, pivot, temp;
    while(l < h) {
        pivot = l;
        i = l;
        j = h;
        while(i < j) {
            while(a[i] <= a[pivot] && i <= h) {
                i++;
            }
            while(a[j] > a[pivot] && j >= l) {
                j--;
            }
            if(i < j) {
                temp = a[i];
                a[i] = a[j];
                a[j] = temp;
            }
        }
        temp = a[j];
        a[j] = a[pivot];
        a[pivot] = temp;
        quick(a, l, j - 1);
        quick(a, j + 1, h);
    }
}

int main() {
    int n, a[100];
    printf("Enter the number of array elements: ");
    scanf("%d", &n);
    printf("Enter the array elements: ");
    read(a, n);
    quick(a, 0, n - 1);
    display(a, n);
    return 0;
}
