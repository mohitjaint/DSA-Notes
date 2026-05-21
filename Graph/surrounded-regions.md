# Surrounded Regions

Problem : [LeetCode - Surrounded Regions](https://leetcode.com/problems/surrounded-regions/description/?utm_source=chatgpt.com)

---

# Problem Idea

Given grid:

```txt id="skf2vu"
X → blocked
O → open
```

Convert:

```txt id="a1k0el"
all completely surrounded O
```

into:

```txt id="u6v9pg"
X
```

Do NOT convert:

```txt id="c0lwwm"
O connected to border
```

---

# Key Observation

Instead of finding:

```txt id="glolzf"
regions to capture
```

Reverse thinking:

Find:

```txt id="1vjpf5"
regions that CANNOT be captured
```

Those are:

```txt id="xixpq8"
border-connected O
```

---

# Core Insight

Any `O` connected to border:

```txt id="98jkpg"
will survive
```

So:

1. Start from border `O`
2. Mark all reachable `O`
3. Convert remaining `O → X`
4. Restore safe cells

---

# Marking Trick

Reuse board.

Temporary state:

```txt id="br7fr7"
S → safe
```

No extra visited matrix.

---

# Algorithm

### Step 1

Traverse border.

If:

```cpp id="mlw2go"
board[i][j]=='O'
```

Mark:

```cpp id="wz01tx"
board[i][j]='S'
```

Push into queue.

---

### Step 2

Run BFS.

Spread only through:

```txt id="wzjlwm"
O
```

Mark all reachable cells:

```cpp id="y4c8yv"
board[ni][nj]='S'
```

---

### Step 3

Traverse entire board.

Convert:

```txt id="mz9p5i"
O → X
```

Restore:

```txt id="abn2kr"
S → O
```

---

# BFS Flow

```txt id="i2svtp"
border O
↓
mark S
↓
BFS expansion
↓
safe region discovered
↓
convert remaining O
```

---

# Optimal Solution

```cpp id="lvt5m0"
class Solution {
public:

void solve(
vector<vector<char>>& board){

int n=
board.size();

int m=
board[0].size();

queue<pair<int,int>>
q;

for(int i=0;i<n;i++){

for(int j=0;j<m;j++){

if(
i==0||
i==n-1||
j==0||
j==m-1){

if(
board[i][j]
=='O'){

board[i][j]
='S';

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

auto [x,y]
=
q.front();

q.pop();

for(int k=0;k<4;k++){

int ni=
x+dr[k];

int nj=
y+dc[k];

if(
ni>=0 &&
ni<n &&
nj>=0 &&
nj<m &&

board[ni][nj]
=='O'){

board[ni][nj]
='S';

q.push(
{ni,nj});
}
}
}

for(int i=0;i<n;i++){

for(int j=0;j<m;j++){

if(
board[i][j]
=='O')

board[i][j]
='X';

else if(
board[i][j]
=='S')

board[i][j]
='O';
}
}
}
};
```

---

# Complexity

Each cell:

- visited at most once

Time:

O(nm)

---

Queue:

O(nm)

---

Extra auxiliary memory:

O(1)

(using board itself)

Optimal.

---

# Why This Is Better Than Component Checking?

Old thinking:

```txt id="3zvkrt"
find region
↓
check border
↓
convert
```

Extra work.

---

Reverse thinking:

```txt id="dh1h8k"
mark guaranteed safe
↓
convert rest
```

Cleaner.

---

# Common Mistakes

❌ Starting BFS from every `O`

---

❌ Using extra visited matrix

---

❌ Forgetting to restore:

```txt id="p6m72q"
S → O
```

---

❌ Converting border-connected cells

---

# Important Insight

This problem teaches:

```txt id="x7r6cm"
Reverse Thinking
+
Grid BFS
+
Boundary Traversal
```

---

# Pattern Recognition

Very common pattern:

```txt id="qg6jlwm"
start from boundary
protect reachable cells
```

Used in:

- Surrounded Regions
- Pacific Atlantic
- Escape Problems
- Island Boundary Problems
- Walls and Gates
