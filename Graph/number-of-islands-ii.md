# Number of Islands II (DSU)

Problem: Count the number of islands after each land-addition operation.

---

# Difference from Number of Islands

### Number of Islands

Grid is already given.

```txt
Run DFS/BFS once
```

Complexity:

O(nm)

---

### Number of Islands II

Land is added dynamically.

```txt
(0,0)
(1,1)
(0,1)
...
```

After every operation:

```txt
Return current island count
```

Re-running DFS after every operation would be too slow.

This is where **DSU shines**.

---

# Key Observation

Initially:

```txt
Water everywhere
```

When a new land cell is added:

```txt
It forms a new island
```

Therefore:

```cpp
cnt++;
```

---

# Another Observation

Suppose new land touches an existing island:

```txt
□ □ □
□ X X
□ □ □
```

The new cell joins that island.

No change in island count.

---

Suppose new land connects two different islands:

```txt
X □ X
□ O □
```

After adding `O`:

```txt
X O X
```

The two islands merge.

Therefore:

```cpp
cnt--;
```

---

# Core DSU Idea

Maintain:

```cpp
int cnt = 0;
```

instead of recomputing islands every time.

---

# Grid to DSU Mapping

For a grid:

```txt
n rows
m columns
```

Convert:

```cpp
(row,col)
```

to:

```cpp
node = row * m + col;
```

---

Example

```txt
n = 3
m = 4
```

Grid:

```txt
(0,0) (0,1) (0,2) (0,3)
(1,0) (1,1) (1,2) (1,3)
(2,0) (2,1) (2,2) (2,3)
```

Nodes:

```txt
0 1 2 3
4 5 6 7
8 9 10 11
```

---

# Common Mistake

Wrong:

```cpp
row * n + col
```

Correct:

```cpp
row * m + col
```

Reason:

```txt
Each row contains m columns
```

---

# Algorithm

For every operation:

---

### Step 1

Handle duplicates.

```cpp
if(mat[x][y] == 1){
    ans.push_back(cnt);
    continue;
}
```

---

### Step 2

Add land.

```cpp
mat[x][y] = 1;
cnt++;
```

New island created.

---

### Step 3

Check 4 neighbors.

```cpp
up
down
left
right
```

---

### Step 4

If neighbor is land:

```cpp
findUPar(node)
!=
findUPar(nei)
```

then:

```cpp
unionBySize(node, nei);
cnt--;
```

Because two islands merged.

---

### Step 5

Store answer.

```cpp
ans.push_back(cnt);
```

---

# Why DSU Works

DSU maintains:

```txt
Connected land cells
```

Whenever two separate islands become connected:

```cpp
unionBySize(...)
```

merges them efficiently.

---

# Example

Operations:

```txt
(0,0)
(0,1)
(1,1)
```

---

Add:

```txt
(0,0)
```

Count:

```txt
1
```

---

Add:

```txt
(0,1)
```

Initially:

```txt
cnt = 2
```

Neighbor exists.

Union:

```txt
cnt = 1
```

Answer:

```txt
1
```

---

Add:

```txt
(1,1)
```

Initially:

```txt
cnt = 2
```

Neighbor exists.

Union:

```txt
cnt = 1
```

Answer:

```txt
1
```

Result:

```txt
[1,1,1]
```

---

# Complexity

Let:

```txt
k = number of operations
```

---

Each operation:

- Check 4 neighbors.
- DSU find/union.

Cost:

O(\alpha(nm))

---

Total:

O(k\alpha(nm))

Practically:

```txt
Almost O(k)
```

because:

```txt
α(nm) < 5
```

for all practical inputs.

---

# Space Complexity

DSU arrays:

O(nm)

Grid:

O(nm)

Overall:

O(nm)

---

# Pattern Recognition

If the problem says:

```txt
Cells are added dynamically
```

and asks:

```txt
Number of connected components after each update
```

Think:

```txt
DSU
```

immediately.

---

# Key Takeaways

1. Maintain island count dynamically.
2. New land:

```cpp
cnt++;
```

3. Merging two different islands:

```cpp
cnt--;
```

4. Use DSU to maintain connectivity.
5. Handle duplicate operations.
6. Grid mapping:

```cpp
node = row * m + col;
```

7. Complexity:

- Time: O(k\alpha(nm))
- Space: O(nm)

---

### DSU Intuition

This is one of the first problems where DSU clearly beats DFS/BFS.

Instead of:

```txt
Recompute all islands after every update
```

we:

```txt
Maintain the answer incrementally
```

which is the main power of DSU in dynamic connectivity problems.
