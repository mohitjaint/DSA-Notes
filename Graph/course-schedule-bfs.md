# Course Schedule I — Kahn's Algorithm (Topological Sort BFS)

Problem : [LeetCode - Course Schedule](https://leetcode.com/problems/course-schedule/description/?utm_source=chatgpt.com)

---

# Problem

Given:

```txt
numCourses
prerequisites
```

Each pair:

```txt
[a,b]
```

means:

```txt
take b before a
```

Need to determine:

```txt
Is it possible to complete all courses?
```

Return:

```txt
true / false
```

---

# Core Idea

A course can only be taken if:

```txt
all prerequisites are completed
```

Instead of repeatedly checking prerequisites:

Store:

```txt
indegree[node]
=
number of prerequisites remaining
```

---

# Graph Representation

Edge:

```txt
prerequisite → course
```

Build:

```cpp
adj[b].push_back(a);
indegree[a]++;
```

Example:

```txt
[1,0]
[2,0]
[3,1]
[3,2]
```

Graph:

```txt
0 → 1
↓
2

↓

3
```

Indegree:

```txt
0 : 0
1 : 1
2 : 1
3 : 2
```

---

# Algorithm

### Step 1 — Build graph + indegree

```txt
adj[]
indegree[]
```

---

### Step 2 — Push all nodes with:

```txt
indegree == 0
```

These can be taken immediately.

---

### Step 3 — BFS

Take node:

```txt
node = q.front()
```

Process it:

```txt
processed++
```

Remove dependency effect:

```cpp
indegree[child]--;
```

If:

```txt
indegree == 0
```

push.

---

### Step 4 — Cycle Detection

If:

```txt
processed
<
numCourses
```

Some courses were never unlocked.

Cycle exists.

Return:

```txt
false
```

Else:

```txt
true
```

---

# Visual Example

Input:

```txt
0 → 1
1 → 2
```

Initial:

```txt
indegree

0 : 0
1 : 1
2 : 1
```

Queue:

```txt
[0]
```

Process:

```txt
0
↓
1
↓
2
```

Processed:

```txt
3
```

Answer:

```txt
true
```

---

Cycle Example

```txt
0 → 1
↑   ↓
└───┘
```

Indegree:

```txt
1 1
```

Queue:

```txt
[]
```

No processing.

Answer:

```txt
false
```

---

# Final Code

```cpp
class Solution {
public:
    bool canFinish(
        int numCourses,
        vector<vector<int>>& prerequisites
    ) {
        vector<vector<int>> adj(numCourses);
        vector<int> indegree(numCourses, 0);

        for (auto& p : prerequisites) {
            int course = p[0];
            int prereq = p[1];

            adj[prereq].push_back(course);
            indegree[course]++;
        }

        queue<int> q;

        for (int i = 0; i < numCourses; i++) {
            if (indegree[i] == 0) {
                q.push(i);
            }
        }

        int processed = 0;

        while (!q.empty()) {
            int node = q.front();
            q.pop();

            processed++;

            for (int child : adj[node]) {
                indegree[child]--;

                if (indegree[child] == 0) {
                    q.push(child);
                }
            }
        }

        return processed == numCourses;
    }
};
```

---

# Complexity

Time:

O(V+E)

Reason:

- Every node processed once
- Every edge processed once

---

Space:

O(V+E)

---

# Pattern Recognition

When you see:

```txt
dependency
order
prerequisite
unlock
schedule
```

Think:

```txt
Topological Sort
```

Two options:

```txt
DFS + path[]

or

Kahn (BFS + indegree)
```

For ordering problems, Kahn often feels cleaner.
