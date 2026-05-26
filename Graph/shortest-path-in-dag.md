# Shortest Path in DAG — Topological Sort + Relaxation

Problem : [GFG - Shortest Path in Directed Acyclic Graph](https://www.geeksforgeeks.org/problems/shortest-path-in-undirected-graph/1?utm_source=chatgpt.com)

---

# Problem

Given:

```txt
Directed Acyclic Graph (DAG)
weighted edges
source = 0
```

Find:

```txt
shortest distance
from source
to every node
```

Return:

```txt
-1
```

for unreachable nodes.

---

# Why not Dijkstra?

Graph is:

```txt
DAG
```

Meaning:

```txt
NO cycles
```

So once a node is processed in:

```txt
topological order
```

its shortest distance becomes final.

No priority queue needed.

---

# Core Idea

Topological order guarantees:

```txt
parent
always comes before
child
```

So when processing:

```txt
u
```

all shortest distances to:

```txt
u
```

are already known.

Then update:

```txt
u → v
```

---

# Algorithm

## Step 1 — Build Graph

Store:

```cpp
adj[u].push_back({v, weight});
```

Example:

```txt
0 → 1 (2)
0 → 4 (1)
1 → 2 (3)
4 → 2 (2)
```

---

## Step 2 — Topological Sort (DFS)

DFS:

Visit children first.

Push node after recursion.

Example:

```txt
0 → 1 → 2
↓
3
```

DFS push:

```txt
2
1
3
0
```

Stack top:

```txt
0
```

Correct processing order.

Code:

```cpp
void topo(
    int node,
    vector<pair<int,int>> adj[],
    vector<int>& vis,
    stack<int>& st
){

    vis[node]=1;

    for(
        auto& [nei,w]
        :
        adj[node]
    ){

        if(
            !vis[nei]
        ){
            topo(
                nei,
                adj,
                vis,
                st
            );
        }
    }

    st.push(node);
}
```

---

## Step 3 — Initialize Distances

```cpp
dist[source]=0
```

Others:

```cpp
INF
```

---

## Step 4 — Relax Edges in Topological Order

Pop stack.

For every edge:

```txt
u → v
```

Update:

```cpp
dist[v]
=
min(
dist[v],
dist[u]+w
)
```

---

### Important Check

Skip unreachable nodes.

Wrong:

```cpp
dist[v]
=
min(
dist[v],
INF+w
)
```

Correct:

```cpp
if(dist[u]==INF)
continue;
```

---

# Example

Graph:

```txt
0 → 1 (2)
↓

4 (1)

1 → 2 (3)

4 → 2 (2)
```

Topo:

```txt
0 4 1 2
```

Process:

```txt
dist[0]=0
```

Relax:

```txt
dist[1]=2
dist[4]=1
```

Process:

```txt
4
```

Update:

```txt
dist[2]=3
```

Process:

```txt
1
```

Update:

```txt
min(3,5)
```

Final:

```txt
0 2 3 1
```

---

# Final Code

```cpp
class Solution {
public:

    void topo(
        int node,
        vector<pair<int,int>> adj[],
        vector<int>& vis,
        stack<int>& st
    ){

        vis[node]=1;

        for(auto& [nei,w]:adj[node]){

            if(!vis[nei]){
                topo(
                    nei,
                    adj,
                    vis,
                    st
                );
            }
        }

        st.push(node);
    }

    vector<int> shortestPath(
        int N,
        int M,
        vector<vector<int>>& edges
    ){

        vector<pair<int,int>> adj[N];

        for(int i=0;i<M;i++){

            int u=edges[i][0];
            int v=edges[i][1];
            int w=edges[i][2];

            adj[u].push_back({v,w});
        }

        vector<int> vis(N);

        stack<int> st;

        for(int i=0;i<N;i++){

            if(!vis[i]){
                topo(
                    i,
                    adj,
                    vis,
                    st
                );
            }
        }

        vector<int> dist(
            N,
            1e9
        );

        dist[0]=0;

        while(!st.empty()){

            int node=st.top();

            st.pop();

            if(
                dist[node]
                ==
                1e9
            ){
                continue;
            }

            for(
                auto& [nei,w]
                :
                adj[node]
            ){

                dist[nei]
                =
                min(
                    dist[nei],
                    dist[node]+w
                );
            }
        }

        for(
            int i=0;
            i<N;
            i++
        ){

            if(
                dist[i]
                ==
                1e9
            ){

                dist[i]=-1;
            }
        }

        return dist;
    }
};
```

---

# Complexity

Topological Sort:

O(V+E)

Relaxation:

O(V+E)

Total:

O(V+E)

Space:

O(V+E)

---

# Pattern Recognition

When you see:

```txt
Shortest Path
+
Weighted
+
DAG
```

Think:

```txt
Topological Sort
+
Relaxation
```

Not:

```txt
Dijkstra
```
