# Network Delay Time — Dijkstra's Algorithm

Problem: [LeetCode - Network Delay Time](https://leetcode.com/problems/network-delay-time/description/?utm_source=chatgpt.com)

---

# Problem

Given:

```txt id="0zjlwm"
times[i] = [u, v, w]
```

Meaning:

```txt id="h6n5pr"
u → v
takes w time
```

A signal starts from:

```txt id="g3r1qv"
node k
```

Find:

```txt id="j8z8yy"
minimum time
for signal to reach
all nodes
```

If some node cannot be reached:

```txt id="hyf8pz"
return -1
```

---

# Key Observation

We need:

```txt id="u0sv79"
shortest time
from source k
to every node
```

This is:

```txt id="rvl7f6"
Single Source Shortest Path
```

Graph:

```txt id="7h1q7i"
Directed
Weighted
Positive Weights
```

So use:

```txt id="n55rr6"
Dijkstra
```

---

# Graph Representation

Store:

```cpp id="9n9r2u"
adj[u].push_back({v, w});
```

Meaning:

```txt id="c0ry1l"
u → v
weight = w
```

---

# Dijkstra

Maintain:

```cpp id="s9d0ym"
dist[node]
```

which stores:

```txt id="s7kg6i"
minimum time
to reach node
```

Initialize:

```cpp id="ez4h7f"
dist[k] = 0
```

Others:

```cpp id="1j8xqj"
INF
```

---

# Priority Queue

Store:

```cpp id="x9k5lw"
{distance, node}
```

Smallest distance first.

```cpp id="vkvk9s"
priority_queue<
    pair<int,int>,
    vector<pair<int,int>>,
    greater<pair<int,int>>
> pq;
```

---

# Relaxation

For edge:

```txt id="jv8qz5"
u → v
weight = w
```

If:

```cpp id="zj2n8u"
dist[u] + w < dist[v]
```

Update:

```cpp id="ok4svx"
dist[v] = dist[u] + w;
```

Push:

```cpp id="s5a2h5"
{dist[v], v}
```

into heap.

---

# Stale Entry Optimization

Queue may contain:

```txt id="vffm84"
(10, node)
(5, node)
```

for the same node.

When:

```txt id="0krp6y"
10
```

comes out later:

Skip it.

```cpp id="yxrjcz"
if(d > dist[u])
    continue;
```

---

# Final Answer

After Dijkstra:

Find:

```txt id="5bljct"
maximum distance
among all nodes
```

because:

```txt id="jovb8j"
signal reaches nodes
at different times
```

The answer is:

```txt id="6i4bxv"
last node reached
```

If any node has:

```txt id="ehx0v6"
INF
```

return:

```txt id="2h3z4z"
-1
```

---

# Example

Input:

```txt id="xj6fd3"
2 → 1 (1)
2 → 3 (1)
3 → 4 (1)

k = 2
```

Shortest times:

```txt id="6lhm3j"
2 = 0
1 = 1
3 = 1
4 = 2
```

Maximum:

```txt id="ykdxwq"
2
```

Answer:

```txt id="dzby8u"
2
```

---

# Final Code

```cpp id="c1x8p9"
class Solution {
public:
    int networkDelayTime(
        vector<vector<int>>& times,
        int n,
        int k
    ) {

        vector<vector<pair<int,int>>> adj(n + 1);

        for (auto &vec : times) {

            int u = vec[0];
            int v = vec[1];
            int w = vec[2];

            adj[u].push_back({v, w});
        }

        priority_queue<
            pair<int,int>,
            vector<pair<int,int>>,
            greater<pair<int,int>>
        > pq;

        vector<int> dist(n + 1, 1e9);

        dist[k] = 0;

        pq.push({0, k});

        while (!pq.empty()) {

            auto [d, u] = pq.top();
            pq.pop();

            if (d > dist[u])
                continue;

            for (auto &[v, w] : adj[u]) {

                if (d + w < dist[v]) {

                    dist[v] = d + w;

                    pq.push({
                        dist[v],
                        v
                    });
                }
            }
        }

        int ans = 0;

        for (int i = 1; i <= n; i++) {

            if (dist[i] == 1e9)
                return -1;

            ans = max(ans, dist[i]);
        }

        return ans;
    }
};
```

---

# Complexity

Vertices:

```txt id="1uvzrq"
V = n
```

Edges:

```txt id="dtav3i"
E = times.size()
```

Time:

O(E\log V)

Space:

O(V+E)

---

# Pattern Recognition

When you see:

```txt id="hn42ks"
weighted graph
+
positive weights
+
shortest path from one source
```

Think:

```txt id="jfdv8e"
Dijkstra
```

When the problem asks:

```txt id="s0xq8f"
time to reach all nodes
```

Think:

```txt id="pnw3sx"
max(shortest distance)
```

after running Dijkstra.
