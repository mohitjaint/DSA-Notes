# Largest Island (DSU)

Problem: [LeetCode 827 - Making A Large Island](https://leetcode.com/problems/making-a-large-island/description/?utm_source=chatgpt.com)

---

# Problem

Given an `n × n` binary grid.

You may change **at most one**:

```txt
0 → 1
```

Return the size of the largest island possible.

---

# Brute Force Idea

For every zero:

```txt
Flip it
Run DFS/BFS
Compute island size
```

Complexity:

O(N^4)

Too slow.

---

# Key Observation

Suppose:

```txt
1 1
1 0
```

The existing island size is:

```txt
3
```

If we flip:

```txt
0 → 1
```

Answer becomes:

```txt
4
```

We don't need to recompute islands.

We only need:

```txt
Size of neighboring islands
```

---

# DSU Idea

Build connected components of all existing:

```txt
1 cells
```

using DSU.

Store:

```txt
Component Size
```

inside DSU.

---

# Grid to DSU Mapping

For an `n × n` grid:

```cpp
node = row * n + col;
```

Example:

```txt
0 1 2
3 4 5
6 7 8
```

Every cell gets a unique DSU node.

---

# Step 1: Build Islands

For every land cell:

```cpp
if(grid[x][y] == 1)
```

check 4 neighbors.

If neighbor is land:

```cpp
unionBySize(node, neighbor);
```

This builds DSU components.

---

# Example

```txt
1 1 0
1 0 0
0 0 1
```

DSU components:

```txt
Island A size = 3
Island B size = 1
```

---

# Step 2: Try Flipping Every Zero

Suppose:

```txt
1 1
1 0
```

At cell:

```txt
(1,1)
```

neighbors:

```txt
up
left
```

Both belong to the same island.

---

# Important Bug

Wrong:

```cpp
currSize += size(up);
currSize += size(left);
```

Result:

```txt
1 + 3 + 3 = 7
```

Wrong.

---

# Deduplicate Parents

Use:

```cpp
unordered_set<int> parents;
```

Store:

```cpp
parents.insert(
    ds.findUPar(neighbor)
);
```

Now each island contributes once.

---

Example:

```txt
1 1
1 0
```

Parents:

```txt
{rootA}
```

Result:

```txt
1 + 3 = 4
```

Correct.

---

# Computing New Island Size

For a zero:

```cpp
currSize = 1;
```

because we flip it.

Then:

```cpp
for(parent : parents)
    currSize += size(parent);
```

Update:

```cpp
ans = max(ans, currSize);
```

---

# Special Case: All Ones

Example:

```txt
1 1
1 1
```

No zero exists.

If we only process zeros:

```txt
Answer remains 0
```

Wrong.

---

Handle by checking existing islands too:

```cpp
ans = max(
    ans,
    ds.sizeOfPar(node)
);
```

for every land cell.

---

# Why DSU Works

DSU maintains:

```txt
Connected Components
```

and:

```txt
Size of each component
```

Therefore:

```txt
Flipping a zero
=
Connecting neighboring components
```

which can be computed instantly.

---

# Complexity

Let:

```txt
Grid = N × N
```

---

### Building DSU

Each cell checks 4 neighbors.

Time:

O(N^2\alpha(N^2))

---

### Evaluating Zeros

Each zero checks:

```txt
4 neighbors
```

Set size ≤ 4.

Time:

O(N^2\alpha(N^2))

---

### Total

O(N^2\alpha(N^2))

Practically:

```txt
O(N²)
```

because:

```txt
α(N²) < 5
```

---

# Space Complexity

DSU arrays:

O(N^2)

Grid:

O(N^2)

Overall:

O(N^2)

---

# Pattern Recognition

Whenever a problem says:

```txt
Change one node
Add one edge
Add one cell
```

and asks:

```txt
What is the largest connected component now?
```

Think:

```txt
DSU + Component Sizes
```

---

# Key Takeaways

### DSU Node

```cpp
node = row * n + col;
```

---

### Build Components

```cpp
unionBySize()
```

for adjacent land cells.

---

### For Every Zero

1. Collect unique neighboring parents.
2. Add their component sizes.
3. Add 1 for the flipped cell.
4. Update answer.

---

### Use Set

```cpp
unordered_set<int> parents;
```

to avoid counting the same island multiple times.

---

### Handle All-Ones Grid

Also consider:

```cpp
ds.sizeOfPar(node)
```

for existing islands.

---

### Complexity

Time:

O(N^2\alpha(N^2))

Space:

O(N^2)

---

### Main DSU Lesson

This problem introduces a very common DSU technique:

```txt
Store component sizes
```

Then instead of traversing components again, you can answer:

```txt
What happens if I connect these components?
```

in nearly constant time.
