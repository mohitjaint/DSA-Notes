# Number of Operations to Make Network Connected

Problem: [LeetCode 1319 - Number of Operations to Make Network Connected](https://leetcode.com/problems/number-of-operations-to-make-network-connected/description/?utm_source=chatgpt.com)

---

# Problem

Given:

```txt id="u48b3q"
n computers
connections between computers
```

In one operation:

```txt id="ttrh7v"
Remove an existing cable
and connect two disconnected computers
```

Find the minimum operations needed to make the entire network connected.

Return:

```txt id="z7tf2t"
-1
```

if impossible.

---

# Key Observation 1

To connect:

```txt id="8axb6x"
n nodes
```

we need at least:

n-1

edges.

Therefore:

```cpp
if(connections.size() < n - 1)
    return -1;
```

---

# Why?

A connected graph with:

```txt id="1n1fca"
n vertices
```

must have at least:

```txt id="e36lsp"
n-1 edges
```

Example:

```txt id="o7l74x"
1 node → 0 edges
2 nodes → 1 edge
3 nodes → 2 edges
4 nodes → 3 edges
```

---

# Key Observation 2

If the graph has:

```txt id="r5z0d4"
k connected components
```

then we need:

k-1

extra connections to connect all components.

Example:

```txt id="a1bpr5"
A
B
C
```

3 components.

Need:

```txt id="i1w0rf"
A-B
B-C
```

Total:

```txt id="bpml9j"
2 operations
```

which is:

```txt id="v33f5d"
3 - 1
```

---

# Approach 1: DFS / Connected Components

---

## Idea

Count the number of connected components.

Answer:

```cpp
components - 1
```

---

## Algorithm

Build adjacency list.

Run DFS from every unvisited node.

Each DFS discovers one component.

```cpp
if(!vis[i]){
    components++;
    dfs(i);
}
```

---

## DFS Code

```cpp
void dfs(
    int node,
    vector<int>& vis,
    vector<vector<int>>& adj
){

    vis[node] = 1;

    for(auto &nei : adj[node]){

        if(!vis[nei])
            dfs(nei, vis, adj);
    }
}
```

---

## Complexity

Building graph:

```txt id="i42tgh"
O(E)
```

DFS:

```txt id="c1umsl"
O(V + E)
```

Overall:

O(V+E)

Space:

O(V+E)

---

# Approach 2: DSU (Disjoint Set Union)

---

## Idea

Instead of explicitly finding components with DFS:

```txt id="34k7fo"
Merge connected nodes
using DSU
```

Then count the number of ultimate parents.

---

## DSU Operations

### Find

Returns the ultimate parent.

```cpp
int findParent(
    int u,
    vector<int>& parent
){

    if(parent[u] == u)
        return u;

    return parent[u]
        =
        findParent(
            parent[u],
            parent
        );
}
```

Uses:

```txt id="ikvpzt"
Path Compression
```

---

### Union By Size

Attach smaller tree under larger tree.

```cpp
void unionBySize(
    int u,
    int v,
    vector<int>& parent,
    vector<int>& size
){
    int pu =
        findParent(u,parent);

    int pv =
        findParent(v,parent);

    if(pu == pv)
        return;

    if(size[pu] < size[pv]){

        parent[pu] = pv;

        size[pv] += size[pu];
    }
    else{

        parent[pv] = pu;

        size[pu] += size[pv];
    }
}
```

---

## Counting Components

After all unions:

```cpp
int components = 0;

for(int i = 0; i < n; i++){

    if(findParent(i,parent) == i)
        components++;
}
```

Each ultimate parent represents one component.

Answer:

```cpp
components - 1
```

---

# DSU Complexity

---

## Without Optimization

Find:

```txt id="d9xz03"
O(N)
```

Union:

```txt id="vov7w7"
O(N)
```

Very slow.

---

## With

```txt id="8lpukr"
Path Compression
+
Union By Size
```

Find:

O(\alpha(N))

Union:

O(\alpha(N))

where:

```txt id="zmnwuv"
α(N)
```

is the inverse Ackermann function.

---

## How Small is α(N)?

| N                     | α(N) |
| --------------------- | ---- |
| 10                    | < 4  |
| 10^6                  | < 5  |
| 10^18                 | < 5  |
| Universe-sized values | < 5  |

Therefore:

```txt id="drn26i"
DSU is practically O(1)
```

---

## Total DSU Complexity

Process all edges:

O(E\alpha(N))

Count components:

O(N\alpha(N))

Overall:

O((N+E)\alpha(N))

Practically:

```txt id="k5m47z"
Almost O(N + E)
```

---

# DFS vs DSU

| Feature              | DFS/BFS | DSU          |
| -------------------- | ------- | ------------ |
| Count Components     | ✅      | ✅           |
| Easy to Code         | ✅      | ❌           |
| Dynamic Connectivity | ❌      | ✅           |
| Used in Kruskal      | ❌      | ✅           |
| Time Complexity      | O(V+E)  | O((V+E)α(N)) |

---

# Pattern Recognition

If problem asks:

```txt id="dn9h2t"
Count Connected Components
```

Use:

```txt id="nkh4rt"
DFS/BFS
or
DSU
```

---

If problem asks:

```txt id="uyxgag"
Repeatedly merge groups
```

Think:

```txt id="rr9g3i"
DSU
```

---

If problem asks:

```txt id="s4lsow"
Minimum Spanning Tree
```

Think:

```txt id="mb83cb"
Kruskal + DSU
```

---

# Key Takeaways

1. Need at least:

n-1

edges to connect `n` nodes.

2. If graph has:

```txt id="k7t1mr"
k components
```

answer is:

k-1

3. Can solve using:
   - DFS/BFS
   - DSU

4. DSU uses:
   - Path Compression
   - Union By Size/Rank

5. DSU complexity:

O((N+E)\alpha(N))

which is effectively linear in practice.

6. This is one of the classic introductory DSU problems and a good precursor to **Kruskal's MST Algorithm**.
