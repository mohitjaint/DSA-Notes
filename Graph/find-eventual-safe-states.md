# Find Eventual Safe States — Reverse Graph + Kahn's Algorithm

Problem : [LeetCode - Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states/description/?utm_source=chatgpt.com)

---

# Problem

Given a directed graph:

```txt id="jlwm1"
graph[i]
=
all nodes reachable from i
```

Return all:

```txt id="wjglwm2"
safe nodes
```

A node is safe if:

```txt id="wjglwm3"
every possible path
eventually ends
```

Meaning:

```txt id="wjglwm4"
cannot reach a cycle
```

---

Example

Input:

```txt id="wjglwm5"
0 → 1
↓

2

1 → 3

3 → 1
```

Cycle:

```txt id="wjglwm6"
1 ↔ 3
```

Safe:

```txt id="wjglwm7"
2
```

Unsafe:

```txt id="wjglwm8"
0
1
3
```

Answer:

```txt id="wjglwm9"
[2]
```

---

# Key Observation

Safe nodes are exactly those that:

```txt id="wjglwm10"
eventually reach terminal nodes
```

Terminal node:

```txt id="wjglwm11"
outdegree = 0
```

Example:

```txt id="wjglwm12"
0 → 1 → 2
```

Node:

```txt id="wjglwm13"
2
```

has:

```txt id="wjglwm14"
outdegree=0
```

Safe.

Then:

```txt id="wjglwm15"
1
```

becomes safe.

Then:

```txt id="wjglwm16"
0
```

becomes safe.

---

# Trick — Reverse Graph

Original:

```txt id="wjglwm17"
u → v
```

Store:

```txt id="wjglwm18"
v → u
```

Code:

```cpp id="wjglwm19"
adj[v].push_back(u);
```

Meaning:

```txt id="wjglwm20"
child knows parent
```

---

# Why Reverse?

Normal Kahn removes:

```txt id="wjglwm21"
indegree
```

Here we remove:

```txt id="wjglwm22"
outdegree
```

because:

```txt id="wjglwm23"
safe
=
remaining outgoing edges = 0
```

---

# Algorithm

### Step 1

Build:

```cpp id="wjglwm24"
reverse graph
```

and:

```cpp id="wjglwm25"
outdegree[]
```

---

### Step 2

Push:

```txt id="wjglwm26"
outdegree==0
```

into queue.

These are terminal.

---

### Step 3

BFS

Take:

```txt id="wjglwm27"
safe node
```

Process its parents.

Remove dependency:

```cpp id="wjglwm28"
outd[parent]--;
```

---

### Step 4

If:

```txt id="wjglwm29"
outdegree becomes 0
```

parent becomes safe.

Push.

---

### Step 5

Sort answer.

---

# Visual Example

Original:

```txt id="wjglwm30"
0 → 1
↓

2

1 → 3

3 → 4
```

Reverse:

```txt id="wjglwm31"
1 → 0
2 → 0
3 → 1
4 → 3
```

Outdegree:

```txt id="wjglwm32"
0:2
1:1
2:0
3:1
4:0
```

Queue:

```txt id="wjglwm33"
[2,4]
```

Process:

```txt id="wjglwm34"
2
↓

0 becomes 1
```

Process:

```txt id="wjglwm35"
4
↓

3 becomes 0
```

Push:

```txt id="wjglwm36"
3
```

Continue.

Safe nodes:

```txt id="wjglwm37"
[2,3,4]
```

---

# Final Code

```cpp
class Solution {
public:
    vector<int> eventualSafeNodes(
        vector<vector<int>>& graph
    ) {
        int n = graph.size();

        vector<int> outdegree(n);

        vector<vector<int>> rev(n);

        for (int i = 0; i < n; i++) {
            outdegree[i] = graph[i].size();

            for (int child : graph[i]) {
                rev[child].push_back(i);
            }
        }

        queue<int> q;

        for (int i = 0; i < n; i++) {
            if (outdegree[i] == 0) {
                q.push(i);
            }
        }

        vector<int> ans;

        while (!q.empty()) {
            int node = q.front();
            q.pop();

            ans.push_back(node);

            for (int parent : rev[node]) {
                outdegree[parent]--;

                if (outdegree[parent] == 0) {
                    q.push(parent);
                }
            }
        }

        sort(ans.begin(), ans.end());

        return ans;
    }
};
```

---

# Complexity

Time:

- Reverse graph → `O(E)`
- BFS → `O(V+E)`
- Sort → `O(V log V)`

Total:

O(V+E+V\log V)

---

Space:

O(V+E)

---

# Alternative Solution

DFS:

```txt id="wjglwm38"
vis[]
path[]
safe[]
```

Cycle detection.

But Kahn is cleaner.

---

# Pattern Recognition

When you see:

```txt id="wjglwm39"
safe states
eventual termination
terminal nodes
```

Think:

```txt id="wjglwm40"
Reverse Graph
+
Kahn's Algorithm
```

This is one of the most useful reverse-graph patterns in graphs.
