# Rotting Oranges

Problem : [LeetCode - Rotting Oranges](https://leetcode.com/problems/rotting-oranges/description/?utm_source=chatgpt.com)

---

# Problem Idea

Grid contains:

```txt id="y3vm8n"
0 → empty cell
1 → fresh orange
2 → rotten orange
```

Every minute:

```txt id="rnkg4s"
rotten oranges infect adjacent fresh oranges
```

(up, down, left, right)

Need:

```txt id="j3kqbe"
minimum time to rot all oranges
```

Return:

```txt id="rt17bx"
-1
```

if impossible.

---

# Key Observation

All rotten oranges spread:

```txt id="z2hsv9"
simultaneously
```

This is NOT normal BFS.

This is:

```txt id="tbwsxp"
multi-source BFS
```

---

# Core Idea

Instead of:

```txt id="2rk2dw"
starting BFS from one rotten orange
```

Push:

```txt id="ptk1vk"
ALL rotten oranges initially
```

Each BFS level represents:

```txt id="aqjcc7"
1 minute
```

---

# BFS Interpretation

Queue stores:

```txt id="qzskdg"
currently rotten oranges
```

Process:

```txt id="yhrlvw"
one BFS level
=
one minute
```

---

# Step 1 — Initialization

Traverse grid.

Count:

```txt id="n9gvjv"
fresh oranges
```

Push:

```txt id="uj8fwx"
all rotten oranges
```

---

# Step 2 — BFS Spread

For every rotten orange:

Check:

```txt id="tx4xv2"
up
down
left
right
```

If fresh:

```cpp id="sq1iux"
grid[nx][ny] = 2;
countOne--;
q.push(...)
```

---

# Step 3 — Final Answer

If:

```txt id="g57vtg"
countOne == 0
```

Return:

```txt id="m7yq8v"
time
```

Else:

```txt id="7fjlwm"
-1
```

---

# Optimal Solution

```cpp id="rnj0r8"
class Solution {
public:

    int orangesRotting(
        vector<vector<int>>& grid) {

        queue<pair<int,int>> q;

        int m = grid.size();

        int n = grid[0].size();

        int countOne = 0;

        for(int i=0;i<m;i++){

            for(int j=0;j<n;j++){

                if(grid[i][j]==2)
                    q.push({i,j});

                if(grid[i][j]==1)
                    countOne++;
            }
        }

        if(!countOne)
            return 0;

        int time = -1;

        while(!q.empty()){

            int size=q.size();

            while(size--){

                auto [i,j]=q.front();

                q.pop();

                if(i-1>=0 &&
                   grid[i-1][j]==1){

                    q.push({i-1,j});

                    grid[i-1][j]=2;

                    countOne--;
                }

                if(i+1<m &&
                   grid[i+1][j]==1){

                    q.push({i+1,j});

                    grid[i+1][j]=2;

                    countOne--;
                }

                if(j-1>=0 &&
                   grid[i][j-1]==1){

                    q.push({i,j-1});

                    grid[i][j-1]=2;

                    countOne--;
                }

                if(j+1<n &&
                   grid[i][j+1]==1){

                    q.push({i,j+1});

                    grid[i][j+1]=2;

                    countOne--;
                }
            }

            time++;
        }

        return countOne
               ? -1
               : time;
    }
};
```

---

# Complexity

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(m × n)   |
| Space  | O(m × n)   |

Optimal.

---

# Why No Visited Array?

Grid itself stores state:

```cpp id="5lmxbg"
grid[x][y] = 2
```

means:

```txt id="jvhksp"
already visited / already rotten
```

No extra memory needed.

---

# Why Count Fresh Oranges?

Instead of final traversal:

```cpp id="r4zvnl"
countOne--
```

tracks remaining fresh oranges.

Avoids extra pass.

---

# Important Insight

Multi-source BFS means:

```txt id="3z0mns"
all sources start together
```

Not:

```txt id="q5mz9h"
one after another
```

---

# Common Mistakes

❌ Running BFS separately for each rotten orange.

---

❌ Using DFS.

Spread happens simultaneously.

---

❌ Forgetting to mark newly rotten immediately.

---

❌ Doing unnecessary final traversal.

Track fresh count instead.

---

# Pattern Recognition

Classic:

```txt id="y6y2nn"
Multi-source BFS
```

Used in:

- Rotting Oranges
- Walls and Gates
- Fire Spread
- Distance to nearest cell
- Infection problems
- Shortest distance from multiple sources
