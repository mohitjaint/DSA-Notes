# Is Graph Bipartite? — DFS Coloring

Problem : [LeetCode - Is Graph Bipartite?](https://leetcode.com/problems/is-graph-bipartite/description/?utm_source=chatgpt.com)

---

# Problem

Given an undirected graph:

```txt id="wjglwm1"
graph[i]
=
neighbors of node i
```

Determine:

```txt id="wjglwm2"
can graph be divided
into 2 groups
```

Such that:

```txt id="wjglwm3"
no adjacent nodes
belong to same group
```

Return:

```txt id="wjglwm4"
true
```

or

```txt id="wjglwm5"
false
```

---

Example

Input:

```txt id="wjglwm6"
0—1
| |
3—2
```

Possible:

```txt id="wjglwm7"
Group A:
0 2

Group B:
1 3
```

Answer:

```txt id="wjglwm8"
true
```

---

Counterexample:

```txt id="wjglwm9"
0
/\
1-2
```

Triangle.

Impossible.

Answer:

```txt id="wjglwm10"
false
```

---

# Key Observation

Bipartite means:

```txt id="wjglwm11"
2-coloring possible
```

Assign:

```txt id="wjglwm12"
Color 0
Color 1
```

Rule:

```txt id="wjglwm13"
neighbors
must have opposite color
```

---

# Graph Coloring Idea

Start:

```txt id="wjglwm14"
node = 0
```

Assign:

```txt id="wjglwm15"
color = 0
```

Neighbors:

```txt id="wjglwm16"
color = 1
```

Their neighbors:

```txt id="wjglwm17"
color = 0
```

Continue.

If:

```txt id="wjglwm18"
same color conflict
```

Graph is not bipartite.

---

Example

Graph:

```txt id="wjglwm19"
0—1
|
2
```

Coloring:

```txt id="wjglwm20"
0 → red

1 → blue

2 → blue
```

Valid.

---

Example (Invalid)

```txt id="wjglwm21"
0
/\
1-2
```

Color:

```txt id="wjglwm22"
0 → red

1 → blue

2 → blue
```

But:

```txt id="wjglwm23"
1 connected 2
```

Conflict.

Return:

```txt id="wjglwm24"
false
```

---

# DFS Approach

Maintain:

```cpp id="wjglwm25"
colors[]
```

Values:

```txt id="wjglwm26"
-1 → unvisited

0 → first color

1 → second color
```

---

DFS:

Assign:

```cpp id="wjglwm27"
colors[node]=color;
```

Visit:

```cpp id="wjglwm28"
neighbor
```

If:

```cpp id="wjglwm29"
uncolored
```

Assign:

```cpp id="wjglwm30"
!color
```

Else:

Check conflict.

---

# Final Solution

```cpp id="wjglwm31"
class Solution {
public:

    void dfs(
        int node,
        int color,
        vector<vector<int>>& graph,
        vector<int>& colors,
        bool& bipartite
    ){

        if(!bipartite)
            return;

        colors[node]
            =
            color;

        for(
            auto& nei
            :
            graph[node]
        ){

            if(
                colors[nei]
                ==
                -1
            ){

                dfs(
                    nei,
                    !color,
                    graph,
                    colors,
                    bipartite
                );
            }

            else if(
                colors[nei]
                ==
                color
            ){

                bipartite
                    =
                    false;

                return;
            }
        }
    }

    bool isBipartite(
        vector<vector<int>>& graph
    ){

        vector<int>
        colors(
            graph.size(),
            -1
        );

        bool bipartite
            =
            true;

        for(
            int i=0;
            i<graph.size();
            i++
        ){

            if(
                colors[i]
                ==
                -1
            ){

                dfs(
                    i,
                    0,
                    graph,
                    colors,
                    bipartite
                );
            }
        }

        return bipartite;
    }
};
```

---

# Why Loop Over All Nodes?

Graph may be:

```txt id="wjglwm32"
disconnected
```

Example:

```txt id="wjglwm33"
0—1

2—3
```

Need:

```txt id="wjglwm34"
DFS from each component
```

---

# Complexity

Let:

- `V` = nodes
- `E` = edges

Time:

O(V+E)

Every:

```txt id="wjglwm35"
node
```

visited once.

Every:

```txt id="wjglwm36"
edge
```

checked once.

---

Space:

Color array:

O(V)

DFS recursion:

Worst:

O(V)

---

# Common Mistakes

❌ Forgetting disconnected graph

---

❌ Using visited only

Need:

```txt id="wjglwm37"
colors
```

not just visited.

---

❌ Checking:

```cpp id="wjglwm38"
colors[nei]
==
colors[node]
```

before assigning.

---

❌ Coloring current node after recursion

Must color first.

---

# Pattern Recognition

Classic:

```txt id="wjglwm39"
Graph Coloring
+
DFS/BFS
```

Used in:

- Bipartite Graph
- Team Partitioning
- Conflict Detection
- Scheduling Constraints
- 2-SAT intuition
