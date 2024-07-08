## Segment Tree

A Segment Tree is a data structure that stores information about array intervals in a tree format. This allows efficient range queries over an array, while still being flexible enough to allow quick modification of the array.

### Key Features

- **Range Queries:** Efficiently find the sum of consecutive array elements \( a[l \dots r] \) or find the minimum element in such a range in \( O(\log n) \) time.
- **Modifications:** Allows modifying the array by replacing one element or changing the elements of a whole subsegment. For example, you can assign all elements \( a[l \dots r] \) to any value or add a value to all elements in the subsegment.
- **Flexibility:** Can handle a wide range of problems and can be easily generalized to larger dimensions, such as two-dimensional Segment Trees for querying subrectangles in a matrix in \( O(\log^2 n) \) time.
- **Memory Efficiency:** Requires only a linear amount of memory. The standard Segment Tree requires \( 4n \) vertices for working on an array of size \( n \).

### Complexity

- **Time Complexity:**
  - Building the tree: \( O(n) \)
  - Querying a range: \( O(\log n) \)
  - Updating an element: \( O(\log n) \)
  
- **Space Complexity:** \( O(n) \)

### Code

```cpp
#include <iostream>
#include <vector>
#include <climits>
#include <cmath>

using namespace std;

int n, q;
vector<int> arr(n);
int max_length, x;
vector<int> st(max_length, INT_MAX);
vector<int> lazy(max_length, 0);

int getmin(int ql, int qh, int l, int r, int pos) {
    if (l >= ql && r <= qh) return st[pos];
    if (l > qh || r < ql) return INT_MAX;
    int mid = (l + r) / 2;
    return min(getmin(ql, qh, l, mid, pos * 2 + 1), getmin(ql, qh, mid + 1, r, pos * 2 + 2));
}

void update(int k, int u, int l, int h, int pos) {
    if (k > h || k < l) return;
    if (l == h) {
        if (l == k) {
            st[pos] = u;
            return;
        }
    }
    int mid = (l + h) / 2;
    update(k, u, l, mid, pos * 2 + 1);
    update(k, u, mid + 1, h, pos * 2 + 2);
    st[pos] = min(st[pos * 2 + 1], st[pos * 2 + 2]);
}

void constructST(int l, int h, int pos) {
    if (l == h) {
        st[pos] = arr[l];
        return;
    }
    int mid = (l + h) / 2;
    constructST(l, mid, pos * 2 + 1);
    constructST(mid + 1, h, pos * 2 + 2);
    st[pos] = min(st[2 * pos + 1], st[2 * pos + 2]);
}

int main() {
    cin >> n >> q;
    x = (int)(ceil(log2(n)));
    max_length = 2 * (int)pow(2, x) - 1;
    arr.resize(n);
    st.resize(max_length, INT_MAX);
    for (int i = 0; i < n; i++) {
        cin >> arr[i];
    }
    constructST(0, n - 1, 0);
    int type;
    while (q--) {
        int type; cin >> type;
        if (type == 1) {
            int k, u; cin >> k >> u;
            update(k - 1, u, 0, n - 1, 0);
        } else {
            int ql, qh; cin >> ql >> qh;
            cout << getmin(ql - 1, qh - 1, 0, n - 1, 0) << endl;
        }
    }
    return 0;
}
