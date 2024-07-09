# A* Search

A* (pronounced "A-star") is a widely-used graph traversal and pathfinding algorithm renowned for its completeness, optimality, and efficiency. It operates by efficiently finding the shortest path in a weighted graph from a given source node to a destination node, leveraging heuristics to guide its search. Despite its space complexity in storing all generated nodes, A* remains a cornerstone in fields requiring optimal pathfinding solutions due to its practical effectiveness and adaptability.[4]

![A Pathfinding](https://upload.wikimedia.org/wikipedia/commons/c/c2/Astarpathfinding.gif)</br>
*Source: https://en.wikipedia.org/wiki/A*_search_algorithm*

## Architecture

<img src="https://github.com/GRSampreeti/Food-Delivery-System.github.io/raw/main/A%20serach.PNG" alt="A* Search Architecture" width="400" height="300"/><br/>
*A* Search Architecture




## Key Characteristics

- **Class**: Search algorithm
- **Data structure**: Graph
- **Worst-case performance**: \( O(\|E\| log \|V\|) = O(b^d) \)
- **Worst-case space complexity**: \( O(b^d) \)

## Code Implementation

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <unordered_map>
#include <unordered_set>
#include <cmath>
#include <limits>
#include <algorithm>
#include <functional> 

// Define a structure to hold coordinates
struct Vec2i {
    int x, y;

    bool operator==(const Vec2i& other) const {
        return x == other.x && y == other.y;
    }

    bool operator!=(const Vec2i& other) const {
        return !(*this == other);
    }

    bool operator<(const Vec2i& other) const {
        return std::tie(x, y) < std::tie(other.x, other.y);
    }
};

// Hash function for Vec2i to use in unordered_map
namespace std {
    template<>
    struct hash<Vec2i> {
        std::size_t operator()(const Vec2i& k) const {
            return ((std::hash<int>()(k.x) ^ (std::hash<int>()(k.y) << 1)) >> 1);
        }
    };
}

using uint = unsigned int;
using CoordinateList = std::vector<Vec2i>;
using HeuristicFunction = std::function<uint(Vec2i, Vec2i)>;

// Manhattan distance heuristic
uint manhattan(Vec2i source, Vec2i target) {
    return static_cast<uint>(10 * (std::abs(source.x - target.x) + std::abs(source.y - target.y)));
}

// Reconstruct the path
CoordinateList reconstruct_path(std::unordered_map<Vec2i, Vec2i>& cameFrom, Vec2i current) {
    CoordinateList total_path = {current};
    while (cameFrom.find(current) != cameFrom.end()) {
        current = cameFrom[current];
        total_path.push_back(current);
    }
    std::reverse(total_path.begin(), total_path.end());
    return total_path;
}

// A* algorithm implementation
CoordinateList A_Star(Vec2i start, Vec2i goal, HeuristicFunction h, const std::vector<std::vector<int>>& grid) {
    auto cmp = [](const std::pair<uint, Vec2i>& left, const std::pair<uint, Vec2i>& right) { return left.first > right.first; };
    std::priority_queue<std::pair<uint, Vec2i>, std::vector<std::pair<uint, Vec2i>>, decltype(cmp)> openSet(cmp);

    std::unordered_map<Vec2i, Vec2i> cameFrom;
    std::unordered_map<Vec2i, uint> gScore;
    std::unordered_map<Vec2i, uint> fScore;
    std::unordered_set<Vec2i> openSetHash;

    gScore[start] = 0;
    fScore[start] = h(start, goal);

    openSet.emplace(fScore[start], start);
    openSetHash.insert(start);


    while (!openSet.empty()) {
        Vec2i current = openSet.top().second;
        openSet.pop();
        openSetHash.erase(current);

        if (current == goal) {
            return reconstruct_path(cameFrom, current);
        }

        for (const auto& dir : directions) {
            Vec2i neighbor = {current.x + dir.x, current.y + dir.y};

            if (neighbor.x < 0 || neighbor.y < 0 || neighbor.x >= grid.size() || neighbor.y >= grid[0].size() || grid[neighbor.x][neighbor.y] == 1) {
                continue; // Skip out-of-bounds or blocked cells
            }

            uint tentative_gScore = gScore[current] + ((dir.x == 0 || dir.y == 0) ? 10 : 14); // 10 for straight, 14 for diagonal

            if (tentative_gScore < gScore[neighbor] || gScore.find(neighbor) == gScore.end()) {
                cameFrom[neighbor] = current;
                gScore[neighbor] = tentative_gScore;
                fScore[neighbor] = tentative_gScore + h(neighbor, goal);

                if (openSetHash.find(neighbor) == openSetHash.end()) {
                    openSet.emplace(fScore[neighbor], neighbor);
                    openSetHash.insert(neighbor);
                }
            }
        }
    }

    return {}; // Return empty path if goal was never reached
}

int main() {
    // Define the grid, 0 = free cell, 1 = obstacle
    std::vector<std::vector<int>> grid = {
        {0, 0, 0, 0, 0},
        {0, 1, 0, 1, 0},
        {0, 1, 0, 1, 0},
        {0, 0, 0, 0, 0},
        {0, 1, 1, 1, 0},
        {0, 0, 0, 0, 0}
    };

    Vec2i start = {0, 0};
    Vec2i goal = {5, 4};

    CoordinateList path = A_Star(start, goal, manhattan, grid);

    std::cout << "Path found:\n";
    for (const auto& coord : path) {
        std::cout << "(" << coord.x << ", " << coord.y << ")\n";
    }

    return 0;
}
```
