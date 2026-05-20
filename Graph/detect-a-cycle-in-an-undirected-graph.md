# Detect Cycle in Undirected Graph (BFS)

Problem : [GeeksforGeeks - Detect cycle in an undirected graph using BFS](https://www.geeksforgeeks.org/detect-cycle-in-an-undirected-graph-using-bfs/?utm_source=chatgpt.com)

---

# Problem Idea

Given:

```txt id="ntvsl0"
undirected graph
```

Determine whether:

```txt id="l7vw1t"
any cycle exists
```

Graph may be:

```txt id="0tr4lm"
disconnected
```

so all components must be checked.

---

# Key Observation

In undirected graph:

When visiting node:

```txt id="h1e7hc"
seeing visited node
```

does NOT always mean cycle.

Because:

```txt id="y4s1xn"
parent edge exists
```

Example:

```txt id="6c5v7l"
0 --- 1
```

From `1`:

seeing `0` again is normal.

Need to ignore parent.

---

# Core Idea — BFS + Parent Tracking

Queue stores:

```txt id="6u98pz"
(currentNode, parent)
```

For every neighbor:

### Not visited

Push into queue.

---

### Already visited

If:

```cpp id="3b4u98"
neighbor != parent
```

Cycle exists.

---

# BFS Flow

```txt id="0l2jwa"
start node
↓
push(source,parent)
↓
pop node
↓
visit neighbors
↓
if visited and not parent
→ cycle
```

---

# Detect Function

```cpp id="ow1c0x"
bool detect(
    int src,
    vector<int> adj[],
    vector<bool>& vis){

    queue<pair<int,int>> q;

    q.push({src,-1});

    vis[src]=true;

    while(!q.empty()){

        auto [node,parent]
            = q.front();

        q.pop();

        for(int el:adj[node]){

            if(!vis[el]){

                vis[el]=true;

                q.push(
                    {el,node});
            }

            else if(
                el!=parent){

                return true;
            }
        }
    }

    return false;
}
```

---

# Complete Solution

```cpp id="h4s7gk"
bool isCycle(
    int V,
    vector<int> adj[]) {

    vector<bool> vis(
        V,false);

    for(int i=0;i<V;i++){

        if(!vis[i]){

            if(
                detect(
                    i,
                    adj,
                    vis))

                return true;
        }
    }

    return false;
}
```

---

# Why Traverse All Nodes?

Graph can be:

```txt id="z4uqc5"
disconnected
```

Example:

```txt id="l1p4qo"
0---1

2---3---4
 \___/
```

Cycle may exist in another component.

---

# Complexity

Adjacency list DFS/BFS complexity:

O(V+E)

---

Space:

O(V)

(queue + visited)

Optimal.

---

# Why O(V + E)?

Each:

- node visited once
- edge processed twice (undirected graph)

Sometimes written as:

O(V+2E)

which simplifies to:

O(V+E)

---

# Important Insight

Cycle condition:

```txt id="zjlzjv"
visited neighbor
+
neighbor ≠ parent
```

means:

```txt id="7f3bb0"
alternative path exists
```

which forms cycle.

---

# Common Mistakes

❌ Returning cycle on any visited node.

Need parent check.

---

❌ Forgetting disconnected graph.

---

❌ Using adjacency matrix complexity accidentally.

This solution assumes:

```txt id="ckeygf"
adjacency list
```

---

# Pattern Recognition

Classic graph pattern:

```txt id="7e8qag"
BFS
+
parent tracking
+
cycle detection
```

Used in:

- Graph validation
- Connectivity checking
- Tree verification
- Component traversal
