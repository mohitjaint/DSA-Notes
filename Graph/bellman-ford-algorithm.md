# Bellman-Ford Algorithm

---

# When to Use?

Use Bellman-Ford when:

```txt
Shortest Path
+
Negative Edge Weights
```

or

```txt
Need to Detect Negative Cycles
```

Unlike Dijkstra:

```txt
Dijkstra fails with negative edges
```

Bellman-Ford handles them correctly.

---

# Core Idea

Instead of greedily selecting the nearest node (like Dijkstra),

Bellman-Ford repeatedly:

```txt
Relax all edges
```

until shortest distances propagate through the graph.

---

# Relaxation

For edge:

```txt
u → v
weight = w
```

If:

```cpp
dist[u] + w < dist[v]
```

then:

```cpp
dist[v] = dist[u] + w;
```

---

# Why V-1 Iterations?

A shortest path can contain at most:

```txt
V - 1 edges
```

because a path with:

```txt
V edges
```

must repeat a vertex (cycle).

Each iteration propagates shortest distances one edge further.

Therefore after:

```txt
V - 1 iterations
```

all shortest paths are finalized.

---

# Algorithm

### Initialization

```cpp
vector<int> dist(V, INF);

dist[S] = 0;
```

---

### Relax All Edges V−1 Times

```cpp
for(int i = 0; i < V - 1; i++){

    for(auto &edge : edges){

        int u = edge[0];
        int v = edge[1];
        int w = edge[2];

        if(dist[u] != INF &&
           dist[u] + w < dist[v]){

            dist[v] = dist[u] + w;
        }
    }
}
```

---

### Negative Cycle Detection

Perform one more relaxation.

```cpp
for(auto &edge : edges){

    int u = edge[0];
    int v = edge[1];
    int w = edge[2];

    if(dist[u] != INF &&
       dist[u] + w < dist[v]){

        return {-1};
    }
}
```

If distance still improves:

```txt
Negative Cycle Exists
```

---

# Why Does Extra Relaxation Detect a Negative Cycle?

After:

```txt
V-1 iterations
```

all shortest paths should already be finalized.

If some distance decreases again:

```txt
Shortest path is becoming shorter indefinitely
```

which is only possible if a:

```txt
Negative Weight Cycle
```

is reachable from the source.

---

# Example

Graph:

```txt
0 --1--> 1
1 --2--> 2
2 --(-5)--> 1
```

Cycle:

```txt
1 → 2 → 1
```

Weight:

```txt
2 + (-5) = -3
```

Every time we traverse the cycle:

```txt
distance decreases by 3
```

Therefore no finite shortest path exists.

Bellman-Ford detects this in the extra iteration.

---

# Complexity

Let:

```txt
V = vertices
E = edges
```

Outer loop:

```txt
V - 1
```

Inner loop:

```txt
E
```

Time:

O(VE)

Space:

O(V)

---

# Bellman-Ford vs Dijkstra

| Feature                  | Dijkstra   | Bellman-Ford |
| ------------------------ | ---------- | ------------ |
| Positive Weights         | ✅         | ✅           |
| Negative Weights         | ❌         | ✅           |
| Negative Cycle Detection | ❌         | ✅           |
| Time Complexity          | O(E log V) | O(VE)        |

---

# Pattern Recognition

### If graph has:

```txt
Positive Weights Only
```

Use:

```txt
Dijkstra
```

---

### If graph contains:

```txt
Negative Edge Weights
```

Use:

```txt
Bellman-Ford
```

---

### If problem asks:

```txt
Detect Negative Cycle
```

Think:

```txt
Bellman-Ford
+
One Extra Relaxation
```

---

# Key Takeaways

1. Bellman-Ford works with negative edges.
2. Relax all edges exactly:

```txt
V - 1 times
```

3. One extra relaxation detects negative cycles.
4. Time complexity:

O(VE)

5. Bellman-Ford is slower than Dijkstra but more powerful.
