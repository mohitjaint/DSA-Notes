# 01 Matrix

Problem : [LeetCode - 01 Matrix](https://leetcode.com/problems/01-matrix/description/?utm_source=chatgpt.com)

---

# Problem Idea

Given matrix:

```txt
0 → source
1 → normal cell
```

For every cell:

Find:

```txt
distance to nearest 0
```

Distance allowed:

```txt
up
down
left
right
```

---

# Key Observation

For a cell containing `1`:

```txt
nearest zero
```

can come from:

- top
- bottom
- left
- right
- any zero

Running BFS from every `1` is expensive.

---

# Brute Force (Bad)

For every `1`:

```txt
run BFS to nearest 0
```

Complexity:

O((nm)^2)

Too slow.

---

# Core Insight — Reverse The Thinking

Instead of:

```txt
every 1 searches for nearest 0
```

Do:

```txt
all 0s spread together
```

This is:

```txt
Multi-source BFS
```

---

# Why Multi-source BFS Works

BFS guarantees:

```txt
first time a cell is reached
=
shortest distance
```

So:

- push all `0`s initially
- expand level by level
- assign distances.

---

# Trick Used

Reuse matrix.

Convert:

```txt
1 → -1
```

Meaning:

```txt
unvisited
```

Keep:

```txt
0 → source
```

Now matrix stores:

- distance
- visited state

No extra arrays needed.

---

# BFS Flow

```txt
push all 0s
↓
pop cell
↓
visit neighbors
↓
if neighbor == -1
    distance = current + 1
↓
push neighbor
```

---

# Optimal Solution

```cpp
class Solution {
public:

    void bfs(
        vector<vector<int>>& mat){

        int n = mat.size();

        int m = mat[0].size();

        queue<pair<int,int>> q;

        for(int i=0;i<n;i++){

            for(int j=0;j<m;j++){

                if(mat[i][j]==0){

                    q.push({i,j});
                }
            }
        }

        int dr[] =
        {-1,1,0,0};

        int dc[] =
        {0,0,-1,1};

        while(!q.empty()){

            auto [i,j]
                = q.front();

            q.pop();

            for(int k=0;k<4;k++){

                int ni =
                    i + dr[k];

                int nj =
                    j + dc[k];

                if(
                    ni>=0 &&
                    ni<n &&
                    nj>=0 &&
                    nj<m &&
                    mat[ni][nj]
                    == -1){

                    mat[ni][nj]
                        =
                        mat[i][j]
                        + 1;

                    q.push(
                        {ni,nj});
                }
            }
        }
    }

    vector<vector<int>>
    updateMatrix(
        vector<vector<int>>& mat){

        int n =
            mat.size();

        int m =
            mat[0].size();

        for(int i=0;i<n;i++){

            for(int j=0;j<m;j++){

                if(mat[i][j])
                    mat[i][j]=-1;
            }
        }

        bfs(mat);

        return mat;
    }
};
```

---

# Complexity

Every cell:

- enters queue once
- processed once

Time:

O(nm)

Space:

O(nm)

Optimal.

---

# Why No Visited Array?

Matrix itself acts as visited.

Visited condition:

```cpp
mat[ni][nj] == -1
```

After assignment:

```cpp
mat[ni][nj] = mat[i][j]+1
```

cell becomes visited automatically.

---

# Important Insight

This problem teaches:

```txt
distance from nearest source
=
multi-source BFS
```

Instead of:

```txt
destination searches source
```

make:

```txt
all sources expand together
```

---

# Common Mistakes

❌ BFS from every `1`

---

❌ Starting from only one `0`

---

❌ Using `min()` updates

BFS already guarantees shortest distance.

---

❌ Marking visited late

Mark immediately.

---

# Pattern Recognition

Classic:

```txt
Multi-source BFS
+
Shortest Distance
```

Used in:

- Rotting Oranges
- Walls and Gates
- Fire Spread
- Nearest Hospital
- Distance to nearest source
