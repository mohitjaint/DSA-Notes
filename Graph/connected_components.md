# Number of Connected Components in Undirected Graph

---

# Approach 1 — DFS

## Key Idea

Each DFS traversal visits one entire connected component.

So:

```txt id="h7jeh8"
number of DFS calls
=
number of connected components
```

---

# Steps

1. Build adjacency list
2. Maintain visited array
3. Start DFS from every unvisited node
4. Increment component count

---

# DFS Template

```cpp id="ys2j4j"
void dfs(int node,
         vector<vector<int>>& adj,
         vector<int>& vis){

    vis[node] = 1;

    for(auto nei : adj[node]){

        if(!vis[nei])
            dfs(nei, adj, vis);
    }
}
```

---

# Main Logic

```cpp id="4d0mru"
int components = 0;

for(int i = 0; i < V; i++){

    if(!vis[i]){

        dfs(i, adj, vis);
        components++;
    }
}
```

---

# Complexity

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(V + E)   |
| Space  | O(V + E)   |

---

# Pattern Recognition

Used in:

- Number of islands
- Provinces
- Flood fill
- Graph traversal

---

# Common Mistakes

❌ Forgetting undirected edge:

```cpp id="n7m2c6"
adj[u].push_back(v);
adj[v].push_back(u);
```

---

❌ Forgetting visited check

---

---

# Approach 2 — DSU / Union Find

## Key Idea

Each component has a representative root/leader.

Merge nodes connected by edges.

Unique roots = connected components.

---

# DSU Operations

## Find

Returns root of node.

```cpp id="ib5jnv"
int find(int x){

    if(parent[x] == x)
        return x;

    return parent[x] = find(parent[x]);
}
```

(Path Compression)

---

## Union

```cpp id="f7jw5w"
ru = find(u)
rv = find(v)

if(ru != rv)
    parent[ru] = rv
```

Always merge ROOTS.

---

# Main Logic

```cpp id="1t8k0p"
for(auto &e : edges){

    int ru = find(e[0]);
    int rv = find(e[1]);

    if(ru != rv)
        parent[ru] = rv;
}
```

---

# Counting Components

```cpp id="a9ecaj"
for(each node)
    unique_roots.insert(find(node))
```

---

# Complexity

With:

- path compression
- union by size/rank

| Metric | Complexity    |
| ------ | ------------- |
| Time   | O(V + E·α(V)) |
| Space  | O(V)          |

Practically:

```txt id="evl2zf"
≈ O(V + E)
```

---

# Important Concepts

## Path Compression

```cpp id="00hvr6"
parent[x] = find(parent[x])
```

Flattens tree.

---

## Union by Size/Rank

Attach smaller tree under larger tree.

Prevents deep chains.

---

# Common Mistakes

❌ Unioning nodes directly

Wrong:

```cpp id="mgb3ya"
parent[v] = u
```

Correct:

```cpp id="4l7qqk"
parent[find(v)] = find(u)
```

---

❌ Counting parents directly

Use:

```cpp id="0zwx1f"
find(i)
```

to get actual root.

---

# DFS vs DSU

| Method  | Best Use                       |
| ------- | ------------------------------ |
| DFS/BFS | Static graph traversal         |
| DSU     | Dynamic connectivity / merging |

---

# Interview Insight

DFS is simpler and usually expected first.

DSU is advanced and useful for:

- Kruskal MST
- Dynamic connectivity
- Merge/group problems
- Network problems
