# Shortest Path in Binary Matrix — BFS

Problem: [LeetCode - Shortest Path in Binary Matrix](https://leetcode.com/problems/shortest-path-in-binary-matrix/description/?utm_source=chatgpt.com)

---

# Problem

Given:

```txt
n × n binary matrix
```

Where:

```txt
0 → open cell
1 → blocked cell
```

Find:

```txt
shortest path
from (0,0)
to (n−1,n−1)
```

Movement allowed:

```txt
8 directions
```

Return:

```txt
path length
```

If impossible:

```txt
-1
```

---

# Key Observation

Every move costs:

```txt
1
```

So this becomes:

```txt
Unweighted Shortest Path
```

Use:

```txt
BFS
```

NOT:

```txt
Dijkstra
```

---

# Why BFS Works

BFS explores:

```txt
distance 1
↓

distance 2
↓

distance 3
```

So:

```txt
first time visiting a cell
=
shortest distance
```

No need to relax again.

---

# Algorithm

### Step 1 — Edge Case

If start or destination blocked:

```cpp
grid[0][0] == 1
||
grid[n-1][n-1] == 1
```

Return:

```txt
-1
```

---

### Step 2 — Distance Matrix

Initialize:

```cpp
dist[n][n]
```

Store:

```txt
-1 → unvisited
```

Set:

```cpp
dist[0][0]=1
```

---

### Step 3 — BFS

Push:

```txt
(0,0)
```

For every popped cell:

Visit:

```txt
8 neighbours
```

If:

```txt
inside grid
AND
open
AND
not visited
```

Then:

```cpp
dist[nx][ny]
=
dist[x][y]+1
```

Push into queue.

---

### Step 4 — Early Return

If destination reached:

Return immediately.

No need to process rest.

---

# 8 Directions

```txt
↖ ↑ ↗
←   →
↙ ↓ ↘
```

Arrays:

```cpp
dr = {-1,-1,-1,0,0,1,1,1}
dc = {-1, 0, 1,-1,1,-1,0,1}
```

---

# Example

Input:

```txt
0 1
1 0
```

Path:

```txt
(0,0)
↓

(1,1)
```

Distance:

```txt
2
```

Output:

```txt
2
```

---

# Final Code

```cpp
class Solution {
public:
    int shortestPathBinaryMatrix(vector<vector<int>>& grid) {

        int n = grid.size();

        if (grid[0][0] == 1 || grid[n - 1][n - 1] == 1)
            return -1;

        vector<vector<int>> dist(
            n,
            vector<int>(n, -1)
        );

        queue<pair<int, int>> q;

        q.push({0, 0});
        dist[0][0] = 1;

        int dr[] = {-1,-1,-1,0,0,1,1,1};
        int dc[] = {-1,0,1,-1,1,-1,0,1};

        while (!q.empty()) {

            auto [x, y] = q.front();
            q.pop();

            if (x == n - 1 && y == n - 1)
                return dist[x][y];

            for (int i = 0; i < 8; i++) {

                int nx = x + dr[i];
                int ny = y + dc[i];

                if (
                    nx >= 0 &&
                    nx < n &&
                    ny >= 0 &&
                    ny < n &&
                    grid[nx][ny] == 0 &&
                    dist[nx][ny] == -1
                ) {

                    dist[nx][ny] =
                        dist[x][y] + 1;

                    q.push({nx, ny});
                }
            }
        }

        return -1;
    }
};
```

---

# Complexity

Time:

O(n^2)

Reason:

```txt
each cell visited once
```

---

Space:

O(n^2)

---

# Pattern Recognition

When you see:

```txt
grid
+
shortest path
+
all edges same cost
```

Think:

```txt
BFS
```

When you see:

```txt
weighted shortest path
```

Think:

```txt
Dijkstra
```
