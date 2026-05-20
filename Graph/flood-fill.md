# Flood Fill

Problem : [LeetCode - Flood Fill](https://leetcode.com/problems/flood-fill/description/?utm_source=chatgpt.com)

---

# Problem Idea

Starting from:

```txt
(sr, sc)
```

Change:

- current pixel
- all connected pixels
- having same original color.

Connected means:

```txt
up
down
left
right
```

Return updated image.

---

# Key Observation

Flood fill spreads only through:

```txt
same original color
```

So first store:

```cpp
int old = image[sr][sc];
```

---

# Important Edge Case

If:

```cpp
old == color
```

Return immediately.

Why?

Because:

```txt
already recolored cells
=
still valid neighbors
```

which causes infinite revisits.

---

# Core Idea — BFS

Use queue.

Queue stores:

```txt
cells waiting to be colored
```

Process:

1. Pop cell
2. Recolor it
3. Push valid neighbors

---

# BFS Flow

```txt
start
↓
push source
↓
pop cell
↓
change color
↓
push neighbors
↓
repeat
```

---

# Neighbor Traversal

Use direction arrays:

```cpp
int di[] = {-1, 1, 0, 0};
int dj[] = {0, 0, -1, 1};
```

Meaning:

| Direction | Change |
| --------- | ------ |
| Up        | (-1,0) |
| Down      | (+1,0) |
| Left      | (0,-1) |
| Right     | (0,+1) |

---

# BFS Template

```cpp
queue<pair<int,int>> q;

q.push({sr,sc});

while(!q.empty()){

    auto [i,j] = q.front();

    q.pop();

    image[i][j] = color;

    for(int k=0;k<4;k++){

        int ni = i + di[k];

        int nj = j + dj[k];

        if(valid &&
           image[ni][nj] == old){

            q.push({ni,nj});
        }
    }
}
```

---

# Why No Visited Array?

Image itself stores state.

After recoloring:

```cpp
image[x][y] = color;
```

means:

```txt
already visited
```

No extra memory needed.

---

# DFS Alternative

Flood Fill can also be solved using:

```txt
DFS
```

Recursive DFS is often shorter.

---

# Complexity

Every cell:

- processed at most once.

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(m × n)   |
| Space  | O(m × n)   |

Optimal.

---

# Important Insight

Flood Fill is basically:

```txt
Grid Traversal
+
Connected Components
+
BFS / DFS
```

---

# Common Mistakes

❌ Forgetting:

```cpp
old == color
```

edge case.

---

❌ Using extra visited array unnecessarily.

---

❌ Traversing diagonals.

Only:

```txt
up
down
left
right
```

---

❌ Recoloring after pushing.

Mark immediately.

---

# Pattern Recognition

Very common pattern.

Used in:

- Number of Islands
- Surrounded Regions
- Rotting Oranges
- Maze Traversal
- Image Processing
- Region Expansion
