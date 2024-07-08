# Kadane's Algorithm for Maximum Sum Submatrix
Kadane’s algorithm for a 1D array can be extended to find the maximum sum submatrix efficiently. The idea is to fix the left and right columns and use Kadane's algorithm to find the maximum sum contiguous rows between these columns.


### Time Complexity:
O(c^2 * r), where c is the number of columns and r is the number of rows in the matrix.

### Space Complexity:
O(r), where r is the number of rows in the matrix.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

void maxMatrixSum(vector<vector<int>>& matrix) {
    int n = matrix.size();
    int m = matrix[0].size();
    int maxsum = INT_MIN;
    int top = 0, bottom = 0, left = 0, right = 0;

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            for (int k = i; k < n; k++) {
                for (int l = j; l < m; l++) {
                    // Calculate sum of submatrix matrix[i..k][j..l]
                    int curr = 0;
                    for (int x = i; x <= k; x++) {
                        for (int y = j; y <= l; y++) {
                            curr += matrix[x][y];
                        }
                    }

                    // Update maximum sum and boundaries if found a larger sum
                    if (curr > maxsum) {
                        maxsum = curr;
                        top = i;
                        left = j;
                        bottom = k;
                        right = l;
                    }
                }
            }
        }
    }

    // Output the results
    cout << "Maximum sum is: " << maxsum << endl;
    cout << "Top-left corner: (" << top << ", " << left << ")" << endl;
    cout << "Bottom-right corner: (" << bottom << ", " << right << ")" << endl;
}

int main() {
    vector<vector<int>> matrix = {
        { 1, 2, -1, -4, -20 },
        { -8, -3, 4, 2, 1 },
        { 3, 8, 10, 1, 3 },
        { -4, -1, 1, 7, -6 }
    };

    maxMatrixSum(matrix);

    return 0;
}
```
