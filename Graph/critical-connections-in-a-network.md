# Critical Connections in a Network (Bridges)

Problem: [LeetCode 1192 - Critical Connections in a Network](https://leetcode.com/problems/critical-connections-in-a-network/description/?utm_source=chatgpt.com)

---

# Algorithm

**Tarjan's Bridge Finding Algorithm**

---

# Approach

**DFS + Discovery Time (`tin`) + Lowest Reachable Time (`low`)**

For every node:

- `tin[node]` = time at which the node is first visited.
- `low[node]` = earliest discovery time reachable from that node/subtree.

During DFS:

- Tree edges propagate `low[]` upwards.
- Back edges update `low[]` using an ancestor's `tin[]`.
- If a child subtree cannot reach the current node or any ancestor, the connecting edge is a bridge.

---

# Pattern Recognition

Keywords:

```txt
Critical Connection
Bridge
Important Edge
Disconnect Graph
Removing Edge Increases Components
Find All Bridges
```

Think:

```txt
Tarjan's Bridge Algorithm
```

---

# Key Observation

Consider an edge:

```txt
u ----- v
```

If the subtree rooted at `v` cannot reach:

```txt
u
or
any ancestor of u
```

then removing:

```txt
(u,v)
```

disconnects the graph.

Therefore:

```txt
(u,v) is a bridge
```

---

# Definitions

## Discovery Time

```cpp
tin[node]
```

Meaning:

```txt
Time when node was first visited
```

---

## Lowest Reachable Time

```cpp
low[node]
```

Meaning:

```txt
Earliest discovery time reachable
from this node/subtree
```

using:

```txt
0 or more tree edges
+
at most one back edge
```

---

# DFS Rules

## Initialization

```cpp
tin[node] = low[node] = timer++;
```

---

## Case 1: Tree Edge

```txt
u ---> v
(v not visited)
```

After DFS returns:

```cpp
low[u] = min(low[u], low[v]);
```

Reason:

```txt
Anything reachable from v
is also reachable from u
```

---

## Case 2: Back Edge

```txt
u ---> ancestor
```

Update:

```cpp
low[u] = min(low[u], tin[ancestor]);
```

### Important

Use:

```cpp
tin[ancestor]
```

NOT:

```cpp
low[ancestor]
```

---

# Why Not `low[ancestor]`?

A back edge:

```txt
u ---> ancestor
```

only guarantees:

```txt
u can reach ancestor
```

It does **not** guarantee that `u` can use every path that contributed to `low[ancestor]`.

Therefore:

```cpp
low[u] = min(low[u], tin[ancestor]);
```

is correct.

---

# Bridge Condition

For a DFS tree edge:

```txt
u ---> v
```

after DFS finishes:

```cpp
if(low[v] > tin[u])
```

then:

```txt
v's subtree cannot reach
u or any ancestor of u
```

Hence:

```txt
(u,v) is a bridge
```

---

# Intuition Example

Graph:

```txt
0
|
1
|
2
```

Discovery:

```txt
tin[0] = 1
tin[1] = 2
tin[2] = 3
```

No back edge exists.

Therefore:

```txt
low[2] = 3
```

Check:

```cpp
low[2] > tin[1]
```

```txt
3 > 2
```

True.

Therefore:

```txt
(1,2) is a bridge
```

---

# Cycle Example

Graph:

```txt
0
|\
| \
1--2
```

Discovery:

```txt
tin[0] = 1
tin[1] = 2
tin[2] = 3
```

Back edge:

```txt
2 ---> 0
```

Update:

```cpp
low[2] = 1;
```

Propagate:

```cpp
low[1] = 1;
```

Check:

```cpp
low[1] > tin[0]
```

```txt
1 > 1
```

False.

Therefore:

```txt
No bridge
```

which is correct because a cycle provides an alternate path.

---

# Solution Template

## DFS Function

```cpp
int timer = 1;

void dfs(int node,
         int parent,
         vector<vector<int>>& adj,
         vector<int>& vis,
         vector<int>& tin,
         vector<int>& low,
         vector<vector<int>>& bridges)
{
    vis[node] = 1;

    tin[node] = low[node] = timer++;

    for(auto &nei : adj[node]){

        if(nei == parent)
            continue;

        if(!vis[nei]){

            dfs(nei,
                node,
                adj,
                vis,
                tin,
                low,
                bridges);

            low[node] =
                min(low[node], low[nei]);

            if(low[nei] > tin[node]){
                bridges.push_back({node, nei});
            }
        }
        else{

            low[node] =
                min(low[node], tin[nei]);
        }
    }
}
```

---

## Driver Code

```cpp
vector<vector<int>> criticalConnections(
    int n,
    vector<vector<int>>& connections)
{
    vector<vector<int>> adj(n);

    for(auto &e : connections){
        int u = e[0];
        int v = e[1];

        adj[u].push_back(v);
        adj[v].push_back(u);
    }

    vector<int> vis(n, 0);
    vector<int> tin(n);
    vector<int> low(n);

    vector<vector<int>> bridges;

    for(int i = 0; i < n; i++){
        if(!vis[i]){
            dfs(i,
                -1,
                adj,
                vis,
                tin,
                low,
                bridges);
        }
    }

    return bridges;
}
```

---

# Common Mistakes

## Mistake 1

Using:

```cpp
low[u] = min(low[u], low[v]);
```

for a back edge.

Wrong.

Correct:

```cpp
low[u] = min(low[u], tin[v]);
```

---

## Mistake 2

Forgetting:

```cpp
if(nei == parent)
    continue;
```

---

## Mistake 3

Building graph incorrectly:

```cpp
for(auto &e : adj)
```

instead of:

```cpp
for(auto &e : connections)
```

---

## Mistake 4

Running DFS only from node `0`.

For a general graph:

```cpp
for(int i = 0; i < n; i++)
```

run DFS for every unvisited node.

---

# Complexity

Time:

O(V+E)

Space:

O(V)

---

# Final Revision Summary

### Algorithm

```txt
Tarjan's Bridge Algorithm
```

### Approach

```txt
DFS + tin[] + low[]
```

### Core Condition

low[child] > tin[parent]

### Rules

```txt
Tree Edge -> use low[child]

Back Edge -> use tin[ancestor]
```

### Complexity

Time:

O(V+E)

Space:

O(V)

### Recognition

```txt
Bridge
Critical Connection
Important Edge
Disconnect Graph
```

↓

```txt
Tarjan's Bridge Algorithm
```
