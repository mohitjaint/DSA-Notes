# DFS and BFS Traversal of Graph

---

# Graph Representation

## Adjacency List

```cpp
vector<vector<int>> adj(V);
```

For edge:

```txt id="r0ll4v"
u -> v
```

```cpp
adj[u].push_back(v);
```

For undirected graph:

```cpp
adj[u].push_back(v);
adj[v].push_back(u);
```

---

# DFS (Depth First Search)

## Key Idea

Go as deep as possible before backtracking.

Uses:

- recursion
  OR
- stack

---

# DFS Template

```cpp
void dfs(int node,
         vector<vector<int>>& adj,
         vector<int>& vis,
         vector<int>& ans){

    vis[node] = 1;

    ans.push_back(node);

    for(auto nei : adj[node]){

        if(!vis[nei])
            dfs(nei, adj, vis, ans);
    }
}
```

---

# DFS Traversal

```cpp
dfs(0, adj, vis, ans);
```

---

# BFS (Breadth First Search)

## Key Idea

Visit nodes level-by-level.

Uses queue.

---

# BFS Template

```cpp
queue<int> q;

q.push(0);
vis[0] = 1;

while(!q.empty()){

    int node = q.front();
    q.pop();

    ans.push_back(node);

    for(auto nei : adj[node]){

        if(!vis[nei]){

            vis[nei] = 1;
            q.push(nei);
        }
    }
}
```

---

# Important BFS Rule

✅ Mark visited BEFORE pushing:

```cpp
vis[nei] = 1;
q.push(nei);
```

---

❌ Wrong

```cpp
push first
mark after pop
```

Can enqueue duplicates.

---

# Complexity

Using adjacency list:

| Traversal | Complexity |
| --------- | ---------- |
| DFS       | O(V + E)   |
| BFS       | O(V + E)   |

---

# Space Complexity

| Structure             | Complexity |
| --------------------- | ---------- |
| adjacency list        | O(V + E)   |
| visited array         | O(V)       |
| recursion stack (DFS) | O(V)       |
| queue (BFS)           | O(V)       |

---

# DFS vs BFS

| DFS                   | BFS                    |
| --------------------- | ---------------------- |
| Goes deep first       | Level order            |
| Uses recursion/stack  | Uses queue             |
| Good for backtracking | Good for shortest path |

---

# Common Mistakes

❌ Forgetting visited check

Causes infinite recursion/repeated nodes.

---

❌ Using adjacency matrix unnecessarily

Matrix complexity:

```txt id="1m0dbq"
O(V²)
```

Adjacency list preferred.

---

❌ BFS visited marking after pop

Can insert same node multiple times.

---

# Traversal Order Note

DFS/BFS order is NOT unique.

Depends on:

- adjacency list order
- edge insertion order
- implementation details.
