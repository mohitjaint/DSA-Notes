# Swim in Rising Water

Problem: [LeetCode 778 - Swim in Rising Water](https://leetcode.com/problems/swim-in-rising-water/description/?utm_source=chatgpt.com)

---

# Problem

You start at:

```txt
(0,0)
```

and want to reach:

```txt
(n-1,n-1)
```

At time `t`, you can stand only on cells with:

```txt
grid[i][j] ≤ t
```

Find the minimum time needed to reach the destination.

---

# Key Observation

The answer is:

```txt
Minimum possible maximum height
encountered along a path
```

Example:

```txt
0 2
1 3
```

Path:

```txt
0 → 1 → 3
```

Maximum height:

```txt
3
```

Answer:

```txt
3
```

---

# Approach 1: Dijkstra (Most Natural)

## State Definition

Instead of:

```txt
Minimum sum of weights
```

define:

```cpp
dist[x][y]
```

as:

```txt
Minimum possible maximum height
needed to reach (x,y)
```

---

## Transition

Suppose current state:

```cpp
dist[x][y]
```

Move to:

```cpp
(nx, ny)
```

New path cost becomes:

```cpp
max(
    dist[x][y],
    grid[nx][ny]
)
```

because the path's cost is the highest height encountered so far.

---

## Relaxation

```cpp
int newCost =
    max(dist[x][y],
        grid[nx][ny]);

if(newCost < dist[nx][ny]){
    dist[nx][ny] = newCost;
    pq.push({newCost, nx, ny});
}
```

---

## Optimizations

### Skip stale entries

```cpp
if(val > dist[x][y])
    continue;
```

---

### Early return

```cpp
if(x == n-1 && y == n-1)
    return val;
```

The first time destination is popped from PQ, answer is finalized.

---

## Complexity

Grid:

```txt
V = n²
E ≈ 4n²
```

Time:

O(E\log V)

=

O(n^2\log n)

Space:

O(n^2)

---

# Approach 2: DSU (Optimal)

## Key Observation

At time `t`:

```txt
All cells with value ≤ t
are accessible.
```

Instead of finding a path:

```txt
Ask:
"When do start and end
become connected?"
```

---

## DSU Modeling

Each cell becomes a DSU node.

Mapping:

```cpp
node = row * n + col;
```

---

## Important Property

The problem guarantees:

```txt
grid contains
0,1,2,...,n²-1
```

exactly once.

So create:

```cpp
vector<pair<int,int>> pos(n*n);
```

Store:

```cpp
pos[grid[i][j]] = {i,j};
```

Now:

```cpp
pos[t]
```

gives the cell whose height is `t`.

---

## Activation Process

For:

```cpp
t = 0 ... n²-1
```

Activate:

```cpp
pos[t]
```

---

### Mark active

```cpp
active[x][y] = 1;
```

---

### Union with active neighbors

```cpp
if(active[nx][ny])
    unionBySize(...)
```

---

### Connectivity Check

After unions:

```cpp
if(
    findUPar(0)
    ==
    findUPar(n*n-1)
)
    return t;
```

The first such `t` is the answer.

---

## Why This Works

At time `t`:

```txt
All cells ≤ t are usable.
```

DSU maintains connectivity among usable cells.

When:

```txt
Source and destination
belong to same component
```

a valid path exists.

---

## Complexity

Each cell:

- Activated once
- Unioned with up to 4 neighbors

Time:

O(n^2\alpha(n^2))

Practically:

```txt
O(n²)
```

because:

```txt
α(n²) < 5
```

---

Space:

O(n^2)

---

# Comparison

| Approach | Time                | Space |
| -------- | ------------------- | ----- |
| Dijkstra | O(n² log n)         | O(n²) |
| DSU      | O(n² α(n²)) ≈ O(n²) | O(n²) |

---

# Which Approach Should You Remember?

### For Interviews

Think:

```txt
Minimum maximum value along a path
```

↓

```txt
Dijkstra
```

This is the most natural solution.

---

### For Optimization

Notice:

```txt
Cells become available
as time increases
```

↓

```txt
Dynamic Connectivity
```

↓

```txt
DSU
```

This gives the optimal complexity.

---

# Pattern Recognition

## Dijkstra Pattern

When path cost is:

```txt
Sum
Maximum
Minimum
Custom monotonic metric
```

Think:

```txt
Modified Dijkstra
```

---

## DSU Pattern

When the problem can be rephrased as:

```txt
At what threshold
do two nodes become connected?
```

Think:

```txt
DSU + Sort/Activate by threshold
```

---

# Key Takeaways

### Dijkstra

State:

```cpp
dist[x][y]
```

means:

```txt
Minimum possible maximum height
to reach (x,y)
```

Transition:

```cpp
max(
    currentCost,
    grid[nx][ny]
)
```

Complexity:

O(n^2\log n)

---

### DSU

Treat each cell as a node.

Activate cells in increasing height order.

Union active neighbors.

First time:

```cpp
findUPar(start)
==
findUPar(end)
```

return current height.

Complexity:

O(n^2\alpha(n^2))

---

### Main Lesson

This problem is a classic example where the same problem can be viewed as:

```txt
Shortest Path
```

or

```txt
Connectivity Under Increasing Threshold
```

leading to two completely different but correct solutions.
