# Number of Enclaves

Problem : [LeetCode - Number of Enclaves](https://leetcode.com/problems/number-of-enclaves/description/?utm_source=chatgpt.com)

---

# Problem Idea

Grid contains:

```txt id="zbw64m"
0 → sea
1 → land
```

Need to count:

```txt id="m8rq8o"
land cells
that cannot reach boundary
```

These are called:

```txt id="tq4ydq"
enclaves
```

Movement allowed:

```txt id="j7w0sy"
up
down
left
right
```

---

# Key Observation

Instead of finding:

```txt id="9fh6fz"
cells trapped inside
```

Reverse thinking:

Find:

```txt id="24ks2h"
cells that can escape
```

Escape means:

```txt id="93tt0t"
connected to border
```

---

# Core Insight

Any land cell connected to border:

```txt id="k6mml8"
cannot be enclave
```

So:

1. Start from border land
2. Remove all reachable land
3. Remaining land = answer

---

# Similarity To Surrounded Regions

Surrounded Regions:

```txt id="pc4hwt"
protect border-connected
```

---

Number of Enclaves:

```txt id="j23nwb"
delete border-connected
```

Same pattern.

---

# Algorithm

### Step 1 — Count Land

Traverse grid.

Count:

```cpp id="7rqmk9"
countOnes++
```

---

### Step 2 — Add Border Land

If:

```cpp id="o90llf"
grid[i][j]==1
```

and on boundary:

Push into queue.

Remove immediately:

```cpp id="y0j9xa"
grid[i][j]=0;
countOnes--;
```

---

### Step 3 — BFS

Process:

```txt id="0mjlwm"
all connected land
```

Remove:

```cpp id="2rvwb4"
grid[ni][nj]=0;
countOnes--;
```

---

### Step 4

Remaining:

```cpp id="wwgg9p"
countOnes
```

is answer.

---

# BFS Flow

```txt id="fwmvmm"
border land
↓
remove
↓
spread
↓
remove connected land
↓
remaining = enclave
```

---

# Optimal Solution

```cpp id="ttwd6u"
class Solution {
public:

int numEnclaves(
vector<vector<int>>& grid){

int m=
grid.size();

int n=
grid[0].size();

int count=0;

queue<pair<int,int>>
q;

for(int i=0;i<m;i++){

for(int j=0;j<n;j++){

if(grid[i][j]){

count++;

if(
i==0||
i==m-1||
j==0||
j==n-1){

grid[i][j]=0;

count--;

q.push(
{i,j});
}
}
}
}

int dr[4]={
-1,1,0,0};

int dc[4]={
0,0,-1,1};

while(!q.empty()){

auto [i,j]
=
q.front();

q.pop();

for(int k=0;k<4;k++){

int ni=
i+dr[k];

int nj=
j+dc[k];

if(
ni>=0 &&
ni<m &&
nj>=0 &&
nj<n &&

grid[ni][nj]
==1){

grid[ni][nj]=0;

count--;

q.push(
{ni,nj});
}
}
}

return count;
}
};
```

---

# Complexity

Each cell:

- visited at most once

Time:

O(mn)

---

Queue:

O(mn)

Optimal.

---

# Why No Visited Array?

Grid itself stores state.

Visited means:

```cpp id="cpxm4y"
grid[i][j]=0
```

No extra memory needed.

---

# Important Insight

Do NOT think:

```txt id="luhx2w"
find trapped cells
```

Think:

```txt id="hf9h3u"
remove escaping cells
```

Reverse thinking simplifies the problem.

---

# Common Mistakes

❌ Starting BFS from every land cell

---

❌ Using DFS separately for every component

---

❌ Forgetting to mark border land immediately

---

❌ Counting cells after BFS

Track count during traversal.

---

# Pattern Recognition

Classic:

```txt id="ajwyfe"
Boundary Traversal
+
Multi-source BFS
+
Reverse Thinking
```

Used in:

- Surrounded Regions
- Number of Enclaves
- Pacific Atlantic
- Escape Problems
- Island Boundary Problems
