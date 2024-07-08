## Ford–Fulkerson Method

The Ford–Fulkerson method finds the maximum flow possible from a starting point to an endpoint in a network of paths. It keeps track of how much more flow each path can handle and repeats the process until no more can be found, giving the maximum flow possible.

### Time Complexity

| Operation        | Average Case      | Worst Case       |
|------------------|-------------------|------------------|
| BFS (single run)  | O(V + E)          | O(V + E)         |
| Ford-Fulkerson   | O(E * max_flow)   | O(E * max_flow)  |

### Space Complexity

| Structure       | Complexity  |
|-----------------|-------------|
| Graph           | O(V^2)      |
| BFS Queue       | O(V)        |
| Parent Array    | O(V)        |

### Code

```cpp
#include <iostream>
#include <limits.h>
#include <queue>
#include <vector>
#include <cstring>

#define V 6 // Number of vertices in graph

bool bfs(int rGraph[V][V], int s, int t, int parent[]) {
    bool visited[V];
    memset(visited, 0, sizeof(visited));
    std::queue<int> q;
    q.push(s);
    visited[s] = true;
    parent[s] = -1;

    while (!q.empty()) {
        int u = q.front();
        q.pop();

        for (int v = 0; v < V; v++) {
            if (visited[v] == false && rGraph[u][v] > 0) {
                if (v == t) {
                    parent[v] = u;
                    return true;
                }
                q.push(v);
                parent[v] = u;
                visited[v] = true;
            }
        }
    }
    return false;
}

int fordFulkerson(int graph[V][V], int s, int t) {
    int u, v;
    int rGraph[V][V]; 
    for (u = 0; u < V; u++)
        for (v = 0; v < V; v++)
            rGraph[u][v] = graph[u][v];

    int parent[V]; 
    int max_flow = 0;

    while (bfs(rGraph, s, t, parent)) {
        int path_flow = INT_MAX;
        for (v = t; v != s; v = parent[v]) {
            u = parent[v];
            path_flow = std::min(path_flow, rGraph[u][v]);
        }

        for (v = t; v != s; v = parent[v]) {
            u = parent[v];
            rGraph[u][v] -= path_flow;
            rGraph[v][u] += path_flow;
        }

        max_flow += path_flow;
    }

    return max_flow;
}

int main() {
    int graph[V][V] = { {0, 16, 13, 0, 0, 0},
                        {0, 0, 10, 12, 0, 0},
                        {0, 4, 0, 0, 14, 0},
                        {0, 0, 9, 0, 0, 20},
                        {0, 0, 0, 7, 0, 4},
                        {0, 0, 0, 0, 0, 0} };

    std::cout << "The maximum possible flow is " << fordFulkerson(graph, 0, 5);
    return 0;
}
```
