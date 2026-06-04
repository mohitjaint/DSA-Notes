# Articulation Points (Cut Vertices)

## Definition

A node whose removal increases the number of connected components.

```txt
Remove node → graph becomes more disconnected
```

---

# Key Idea (Tarjan's Algorithm)

Use DFS and maintain:

```cpp
tin[node]  // discovery time
low[node]  // lowest reachable discovery time
```

---

# Meaning of `low[node]`

```txt
Earliest ancestor reachable
from node's subtree
(using tree edges + at most one back edge)
```

---

# DFS Initialization

```cpp
tin[node] = low[node] = timer++;
```

---

# Cases During DFS

## 1. Tree Edge (Unvisited Child)

```cpp
dfs(child);

low[node] = min(low[node], low[child]);
```

---

## 2. Back Edge (Visited Node, Not Parent)

```cpp
low[node] = min(low[node], tin[child]);
```

⚠️ Use `tin[child]`, NOT `low[child]`.

---

# Articulation Point Conditions

## Case 1: Non-root Node

```cpp
low[child] >= tin[node]
```

Then:

```txt
child subtree cannot reach
any ancestor of node
```

Removing `node` disconnects the child subtree.

So:

```cpp
articulation[node] = true;
```

---

## Case 2: Root Node

```cpp
DFS children > 1
```

Then root is an articulation point.

Example:

```txt
    0
   / \
  1   2
```

Removing `0` separates `1` and `2`.

---

# Template

```cpp
void dfs(int node, int parent){

    vis[node] = 1;

    tin[node] = low[node] = timer++;

    int children = 0;

    for(auto child : adj[node]){

        if(child == parent)
            continue;

        if(!vis[child]){

            children++;

            dfs(child, node);

            low[node] = min(low[node], low[child]);

            if(parent != -1 &&
               low[child] >= tin[node]){

                articulation[node] = 1;
            }
        }
        else{

            low[node] =
                min(low[node], tin[child]);
        }
    }

    if(parent == -1 &&
       children > 1){

        articulation[node] = 1;
    }
}
```

---

# Disconnected Graph

Always run DFS from every unvisited node:

```cpp
for(int i = 0; i < n; i++){

    if(!vis[i])
        dfs(i, -1);
}
```

---

# Complexity

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(V + E)   |
| Space  | O(V)       |

Optimal.

---

# Common Mistakes

❌ Start DFS only from node 0

✅ Run DFS for all components.

---

❌ Forget root special case

```cpp
children > 1
```

---

❌ Use:

```cpp
low[node] = min(low[node], low[child]);
```

for back edge.

✅ Correct:

```cpp
low[node] = min(low[node], tin[child]);
```

---

# Relationship with Bridges

## Articulation Point

```cpp
low[child] >= tin[node]
```

---

## Bridge

```cpp
low[child] > tin[node]
```

Only difference:

```txt
>=   (Articulation Point)
>
   (Bridge)
```

---

# Pattern Recognition

Whenever you see:

```txt
critical node
remove vertex
graph disconnects
```

Think:

```txt
Articulation Points
(tin + low)
```
