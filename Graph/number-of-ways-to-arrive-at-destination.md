# Number of Ways to Arrive at Destination — Dijkstra + Path Counting

Problem: [LeetCode - Number of Ways to Arrive at Destination](https://leetcode.com/problems/number-of-ways-to-arrive-at-destination/description/?utm_source=chatgpt.com)

---

# Problem

Given:

```txt
Undirected Weighted Graph
```

Find:

```txt
Number of shortest paths
from node 0
to node n-1
```

Return answer modulo:

```txt
1e9 + 7
```

---

# First Observation

If the problem only asked:

```txt
Shortest Distance
```

we would use:

```txt
Dijkstra
```

because:

```txt
Weighted Graph
+
Positive Weights
```

---

# New Twist

Now we need:

```txt
Shortest Distance
+
How many shortest paths exist
```

So besides:

```cpp
dist[node]
```

we also maintain:

```cpp
ways[node]
```

---

# Meaning of Arrays

### dist[node]

```txt
Minimum distance
from source
to node
```

---

### ways[node]

```txt
Number of shortest paths
from source
to node
```

---

# Initialization

Source:

```cpp
dist[0] = 0;
ways[0] = 1;
```

Meaning:

```txt
One way to reach source
with distance 0
```

---

# Dijkstra State

Priority Queue stores:

```cpp
{distance, node}
```

Smallest distance comes first.

```cpp
priority_queue<
    pair<long long,int>,
    vector<pair<long long,int>>,
    greater<pair<long long,int>>
> pq;
```

---

# Relaxation Rules

Let:

```txt
u → v
weight = w
```

and

```cpp
newDist = dist[u] + w;
```

---

## Case 1: Better Path Found

```cpp
if(newDist < dist[v])
```

Update:

```cpp
dist[v] = newDist;
ways[v] = ways[u];
```

Why?

Because:

```txt
all previous shortest paths
to v
are no longer shortest
```

Only paths through `u` matter now.

---

Example:

```txt
0 → 1 → 3

distance = 10
```

Suppose later:

```txt
0 → 2 → 3

distance = 7
```

Then:

```txt
distance 10 paths
become useless
```

Replace count.

---

## Case 2: Another Shortest Path Found

```cpp
if(newDist == dist[v])
```

Update:

```cpp
ways[v] =
(
    ways[v]
    +
    ways[u]
) % MOD;
```

Why?

Because we discovered:

```txt
one more shortest route
to v
```

---

Example:

```txt
0 → 1 → 3
0 → 2 → 3
```

Both have:

```txt
distance = 5
```

Then:

```txt
ways[3] = 2
```

---

# Stale Entry Optimization

Priority queue may contain:

```txt
(10,node)
(5,node)
```

for the same node.

When:

```txt
10
```

comes out:

Skip it.

```cpp
if(d > dist[u])
    continue;
```

---

# Example

Graph:

```txt
0 --1-- 1
|       |
1       1
|       |
2 --1-- 3
```

Shortest paths:

```txt
0 → 1 → 3
0 → 2 → 3
```

Distance:

```txt
2
```

Number of shortest paths:

```txt
2
```

Answer:

```txt
ways[3] = 2
```

---

# Final Code

```cpp
class Solution {
public:
    int M = 1e9 + 7;

    int countPaths(int n, vector<vector<int>>& roads) {

        vector<vector<pair<long long,long long>>> adj(n);

        for(auto &vec : roads){

            int u = vec[0];
            int v = vec[1];
            int t = vec[2];

            adj[u].push_back({v,t});
            adj[v].push_back({u,t});
        }

        priority_queue<
            pair<long long,long long>,
            vector<pair<long long,long long>>,
            greater<pair<long long,long long>>
        > pq;

        vector<long long> dist(n, LLONG_MAX);
        vector<long long> ways(n);

        dist[0] = 0;
        ways[0] = 1;

        pq.push({0,0});

        while(!pq.empty()){

            auto [d,u] = pq.top();
            pq.pop();

            if(d > dist[u])
                continue;

            for(auto &[v,t] : adj[u]){

                long long newDist = d + t;

                if(newDist < dist[v]){

                    dist[v] = newDist;

                    ways[v] = ways[u];

                    pq.push({dist[v], v});
                }

                else if(newDist == dist[v]){

                    ways[v] =
                        (ways[v] + ways[u]) % M;
                }
            }
        }

        return ways[n-1];
    }
};
```

---

# Complexity

Dijkstra:

Time:

O(E\log V)

Space:

O(V+E)

---

# Pattern Recognition

When you see:

```txt
Weighted Graph
+
Positive Weights
+
Number of Shortest Paths
```

Think:

```txt
Dijkstra
+
dist[]
+
ways[]
```

### Golden Rule

```txt
Better distance
→ replace ways

Same distance
→ add ways
```

That's the entire trick behind this problem.
