Earliest Deadline First
Whenever a scheduling event occurs (task finishes, new task released, etc.) the queue will be searched for the process closest to its deadline.

Space Complexity Analysis

| Operation              | Complexity | Justification                                                    |
|------------------------|------------|------------------------------------------------------------------|
| Sorting orders         | O(n log n) | Sorting n orders based on deadlines using efficient sorting.      |
| Creating schedule      | O(n)       | Storing the resulting schedule of up to n orders.                |
| Additional space usage | O(n)       | Space for storing the vector of orders and the resulting schedule.|

Time Complexity Analysis

| Operation              | Complexity | Justification                                                    |
|------------------------|------------|------------------------------------------------------------------|
| Sorting orders         | O(n log n) | Sorting n orders based on deadlines using efficient sorting.      |
| Creating schedule      | O(n)       | Iterating through the n sorted orders to create a feasible schedule.|
