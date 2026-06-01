# Most Stones Removed with Same Row or Column

Problem: [LeetCode 947 - Most Stones Removed with Same Row or Column](https://leetcode.com/problems/most-stones-removed-with-same-row-or-column/description/?utm_source=chatgpt.com)

---

# Problem

A stone can be removed if:

```txt id="6t3ozk"
Another stone exists
in the same row
or same column
```

Find the maximum number of stones that can be removed.

---

# Key Observation

Suppose a connected component contains:

```txt id="rqx7ar"
k stones
```

Then we can remove:

```txt id="l8p6es"
k - 1 stones
```

because:

```txt id="1zk8e0"
At least one stone
must remain
```

---

Example:

```txt id="zllwkr"
● ● ● ●
```

All connected.

Can remove:

```txt id="7v4x7l"
3 stones
```

and leave:

```txt id="g64w0f"
1 stone
```

---

# Main Formula

If there are:

```txt id="mlbgd6"
N stones
C connected components
```

Then:

Answer=N-C

---

# The Hard Part

The challenge is NOT DSU.

The challenge is:

```txt id="s6i2iw"
How do we model the graph?
```

---

# Brilliant Observation

Treat:

```txt id="2vhy5r"
Rows as nodes
Columns as nodes
```

A stone:

```txt id="0x8xmr"
(row, col)
```

becomes an edge:

```txt id="ul9x9m"
row node
   |
 stone
   |
column node
```

---

Example

Stones:

```txt id="d2d7xv"
(0,0)
(0,2)
(1,2)
```

Graph:

```txt id="q6h9ks"
R0 -- C0
 |
 C2
 |
R1
```

Everything is connected.

Therefore:

```txt id="c75y8s"
1 connected component
```

---

# Why Offset Columns?

Suppose:

```txt id="4if0eo"
row = 2
col = 2
```

Without offset:

```txt id="dxbcck"
Both become node 2
```

which is wrong.

---

Use:

```cpp id="jqj23n"
col = col + maxRow + 1;
```

Now:

```txt id="ok55q9"
Rows:
0 ... maxRow

Columns:
maxRow+1 ...
```

No collision.

---

# Example

Suppose:

```txt id="qq71o0"
maxRow = 4
```

Then:

```txt id="0b91vp"
Column 0 → 5
Column 1 → 6
Column 2 → 7
...
```

Rows and columns occupy separate ranges.

---

# DSU Construction

For every stone:

```cpp id="xsd10o"
union(row, col)
```

Meaning:

```txt id="m5u3s2"
This row and column
belong to the same component
```

---

# Why Store Used Nodes?

Suppose:

```txt id="tx5xvk"
maxRow = 10000
maxCol = 10000
```

but only:

```txt id="yk5mkj"
5 stones
```

exist.

Most DSU nodes are unused.

---

Store only:

```cpp id="qg2s71"
unordered_set<int> nodes;
```

containing rows/columns that actually appear.

---

# Counting Components

After all unions:

```cpp id="e7vz0m"
for(auto node : nodes)
```

Count roots:

```cpp id="az9kxy"
if(findUPar(node) == node)
    components++;
```

Each root represents one connected component.

---

# Final Answer

```cpp id="y2pgdd"
stones.size() - components
```

From:

N-C

---

# Important Bug I Encountered

### Wrong

```cpp id="8jpwws"
DisjointSet ds(maxRow + maxCol);
```

---

Largest column node is:

```cpp id="9q8kbw"
maxCol + maxRow + 1
```

which exceeds DSU size.

Result:

```txt id="d9dgl7"
heap-buffer-overflow
```

inside:

```cpp id="m03u5n"
findUPar()
```

---

### Correct

```cpp id="v3n6jm"
DisjointSet ds(maxRow + maxCol + 1);
```

---

# DSU Complexity

With:

```txt id="7xkjrz"
Path Compression
+
Union By Size
```

Find:

O(\alpha(N))

Union:

O(\alpha(N))

where:

```txt id="cpbq95"
α(N)
```

is the inverse Ackermann function.

---

# How Small is α(N)?

| N           | α(N) |
| ----------- | ---- |
| 10          | < 4  |
| 10⁶         | < 5  |
| 10¹⁸        | < 5  |
| Huge values | < 5  |

So DSU is practically:

```txt id="0d1nsb"
O(1)
```

per operation.

---

# Total Complexity

For:

```txt id="9bf8ik"
N stones
```

Each stone performs one union.

Time:

O(N\alpha(N))

Practically:

```txt id="ob2yj3"
Almost O(N)
```

---

Space:

O(maxRow+maxCol)

---

# Pattern Recognition

This problem teaches an important DSU modeling trick:

### Objects are not always DSU nodes.

Here:

```txt id="x4v4bz"
Stones are NOT nodes
```

Instead:

```txt id="7n7w4a"
Rows = nodes
Columns = nodes
Stone = edge
```

---

# Key Takeaways

1. In a component with `k` stones, remove:

```txt id="s95yq4"
k - 1 stones
```

2. Answer:

N-C

where:

- `N` = total stones
- `C` = connected components

3. Model:
   - Row = node
   - Column = node
   - Stone = edge

4. Use column offset:

```cpp id="i5l97i"
col += maxRow + 1;
```

5. Count DSU roots among used nodes.

6. Complexity:

- Time: O(N\alpha(N))
- Space: O(maxRow+maxCol)

This is one of the most important DSU modeling problems and often the first place where DSU feels more like a graph abstraction tool than just a connected-components data structure.
