## Hungarian Algorithm

The Hungarian method is an algorithm used to solve the assignment problem, aiming to find the optimal way to assign tasks to workers while minimizing total costs, with a time complexity of \(O(n^3)\).

### Code

```cpp
#include <iostream>
#include <vector>
#include <limits>

using namespace std;

const int INF = numeric_limits<int>::max();

void hungarianAlgorithm(const vector<vector<int>>& costMatrix) {
    int n = costMatrix.size();      // Number of rows (workers/customers)
    int m = costMatrix[0].size();   // Number of columns (tasks/jobs/coupons)

    vector<int> u(n + 1), v(m + 1), p(m + 1), way(m + 1);
    for (int i = 1; i <= n; ++i) {
        p[0] = i;
        int j0 = 0;
        vector<int> minv(m + 1, INF);
        vector<bool> used(m + 1, false);
        do {
            used[j0] = true;
            int i0 = p[j0], delta = INF, j1;
            for (int j = 1; j <= m; ++j) {
                if (!used[j]) {
                    int cur = costMatrix[i0 - 1][j - 1] - u[i0] - v[j];
                    if (cur < minv[j]) {
                        minv[j] = cur;
                        way[j] = j0;
                    }
                    if (minv[j] < delta) {
                        delta = minv[j];
                        j1 = j;
                    }
                }
            }
            for (int j = 0; j <= m; ++j) {
                if (used[j]) {
                    u[p[j]] += delta;
                    v[j] -= delta;
                } else {
                    minv[j] -= delta;
                }
            }
            j0 = j1;
        } while (p[j0] != 0);

        do {
            int j1 = way[j0];
            p[j0] = p[j1];
            j0 = j1;
        } while (j0);
    }

    // Output the optimal assignment and cost
    cout << "Optimal assignment:" << endl;
    for (int j = 1; j <= m; ++j) {
        cout << "Task " << j << " assigned to Worker " << p[j] << endl;
    }
    int cost = -v[0];
    cout << "Optimal cost: " << cost << endl;
}

int main() {
    // Example usage for workforce scheduling (shift assignment)
    vector<vector<int>> costMatrix1 = {
        {3, 1, 4},   // Employee 1 preferences for shifts 1, 2, 3
        {2, 0, 5},   // Employee 2 preferences for shifts 1, 2, 3
        {1, 2, 6}    // Employee 3 preferences for shifts 1, 2, 3
    };

    // Example usage for promotions and discounts (coupon distribution)
    vector<vector<int>> costMatrix2 = {
        {3, 1, 4},   // Customer 1 likelihood for coupons 1, 2, 3
        {2, 0, 5},   // Customer 2 likelihood for coupons 1, 2, 3
        {1, 2, 6}    // Customer 3 likelihood for coupons 1, 2, 3
    };

    cout << "Workforce Scheduling Example:" << endl;
    hungarianAlgorithm(costMatrix1);

    cout << "\nPromotions and Discounts Example:" << endl;
    hungarianAlgorithm(costMatrix2);

    return 0;
}
```
