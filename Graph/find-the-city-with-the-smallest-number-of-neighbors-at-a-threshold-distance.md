# Find the City With the Smallest Number of Neighbors at a Threshold Distance

Problem: [LeetCode 1334 - Find the City With the Smallest Number of Neighbors at a Threshold Distance](https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/description/?utm_source=chatgpt.com)

---

# Problem

Given:

```txt
Undirected weighted graph
Distance threshold
```

Find the city that can reach the **fewest** cities within the threshold distance.

If multiple cities have the same count:

```txt
Return the city with the largest index
```

---

# Key Observation

For every city:

```txt
Need shortest distance
to every other city
```

This means:

```txt
All-Pairs Shortest Path
```

which immediately suggests:

```txt
Floyd-Warshall
```

---

# Graph Representation

Create a distance matrix:

```cpp
vector<vector<int>> dist(
    n,
    vector<int>(n, INF)
);
```

For every edge:

```cpp
dist[u][v] = w;
dist[v][u] = w;
```

because the graph is:

```txt
Undirected
```

---

# Initialization

Distance from a node to itself:

```cpp
dist[i][i] = 0;
```

---

# Floyd-Warshall

For every intermediate node:

```cpp
for(int k = 0; k < n; k++)
```

try improving:

```txt
i → j
```

using:

```txt
i → k → j
```

Recurrence:

dist[i][j]=\min(dist[i][j],dist[i][k]+dist[k][j])

Implementation:

```cpp
for(int k = 0; k < n; k++) {

    for(int i = 0; i < n; i++) {

        for(int j = 0; j < n; j++) {

            dist[i][j] =
                min(
                    dist[i][j],
                    dist[i][k] + dist[k][j]
                );
        }
    }
}
```

---

# Counting Reachable Cities

For every city:

```cpp
int cnt = 0;
```

Count cities satisfying:

```cpp
dist[i][j] <= threshold
```

---

# Tie Breaking

Problem statement:

```txt
Minimum count
If tie → largest city index
```

Use:

```cpp
if(cnt <= bestCount)
```

not:

```cpp
if(cnt < bestCount)
```

Why?

Suppose:

```txt
City 2 -> count = 3
City 4 -> count = 3
```

Using:

```cpp
<=
```

replaces city 2 with city 4.

Thus we keep the larger index.

---

# Example

Suppose:

```txt
City 0 -> 4 reachable
City 1 -> 3 reachable
City 2 -> 3 reachable
City 3 -> 5 reachable
```

Minimum count:

```txt
3
```

Cities:

```txt
1 and 2
```

Answer:

```txt
2
```

because:

```txt
largest index wins
```

---

# Complexity

Distance matrix:

```txt
n × n
```

Space:

O(V^2)

Floyd-Warshall:

Three nested loops:

O(V^3)

---

# Why Not Dijkstra?

You could run Dijkstra from every node:

```txt
V times
```

Complexity:

O(VE\log V)

For this problem:

```txt
n ≤ 100
```

Floyd-Warshall is simpler and efficient enough.

---

# Pattern Recognition

### Single Source Shortest Path

Use:

```txt
Dijkstra
Bellman-Ford
```

---

### All-Pairs Shortest Path

Use:

```txt
Floyd-Warshall
```

---

### This Problem

```txt
All-Pairs Shortest Path
+
Count nodes within threshold
+
Tie-break by largest index
```

---

# Key Takeaways

1. Need distances between **every pair** of cities.
2. Use **Floyd-Warshall**.
3. Core recurrence:

dist[i][j]=\min(dist[i][j],dist[i][k]+dist[k][j])

4. Count cities with:

```cpp
dist[i][j] <= threshold
```

5. For ties:

```cpp
if(cnt <= bestCount)
```

to keep the city with the **largest index**.

6. Complexity:

- Time: O(V^3)
- Space: O(V^2)

This is one of the most common applications of Floyd-Warshall in interviews and coding contests.
