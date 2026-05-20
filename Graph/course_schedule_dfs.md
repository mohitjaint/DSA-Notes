# Course Schedule — Detect Cycle in Directed Graph (DFS)

Problem : [LeetCode - Course Schedule](https://leetcode.com/problems/course-schedule/description/?utm_source=chatgpt.com)

---

# Problem Idea

Given:

```txt id="9r6j0d"
numCourses
```

and:

```txt id="n1syiu"
prerequisites
```

Determine whether:

```txt id="lmx4qz"
all courses can be completed
```

---

# Graph Representation

If:

```txt id="kgt0s2"
[a, b]
```

means:

```txt id="vlvqf3"
take b before a
```

Create edge:

```txt id="xfv4bd"
b → a
```

Example:

```txt id="9d77li"
[[1,0],
 [2,1]]
```

Graph:

```txt id="6ozq1u"
0 → 1 → 2
```

---

# Key Observation

Courses can be completed only if:

```txt id="hvspsk"
directed graph has NO cycle
```

Cycle means:

```txt id="uj6czq"
course depends on itself
```

Impossible.

---

# Why Visited Alone Fails

Example:

```txt id="e72knb"
0 → 1
↓
2
↑
```

Visiting a node again:

```txt id="nuv2mx"
does NOT always mean cycle
```

Need to know:

```txt id="r7zqfh"
is node still in current DFS path?
```

---

# Core Idea

Use two arrays:

---

## `vis[]`

Meaning:

```txt id="b0mjlwm"
Has node ever been visited?
```

Once visited:

```cpp id="q2wzci"
vis[node]=true
```

never reset.

---

## `path[]`

Meaning:

```txt id="upbz8v"
Is node currently inside recursion stack?
```

Before DFS:

```cpp id="lzcj90"
path[node]=true;
```

After DFS completes:

```cpp id="x2qci2"
path[node]=false;
```

---

# Cycle Detection Rule

If:

```cpp id="ncz6g0"
path[neighbor]
```

is true:

means:

```txt id="r0q16h"
current DFS reached itself
```

Cycle exists.

---

# DFS Logic

```cpp id="lvdh3u"
bool dfs(node)
```

returns:

```txt id="ox1jqx"
true → cycle exists
false → no cycle
```

---

# DFS Flow

```txt id="o5m8mx"
mark visited
↓
mark current path
↓
visit neighbors
↓
if neighbor already in path
→ cycle
↓
remove node from path
```

---

# Template

```cpp id="prz2zn"
bool dfs(
    int src,
    vector<bool>& vis,
    vector<bool>& path,
    vector<int> adj[]){

    vis[src]=true;

    path[src]=true;

    for(int nei:adj[src]){

        if(!vis[nei]){

            if(
                dfs(
                    nei,
                    vis,
                    path,
                    adj))

                return true;
        }

        else if(path[nei]){

            return true;
        }
    }

    path[src]=false;

    return false;
}
```

---

# Main Function

```cpp id="whqjbg"
bool canFinish(
    int numCourses,
    vector<vector<int>>& prerequisites){

    vector<int>
        adj[numCourses];

    for(auto& p:
        prerequisites){

        adj[p[1]]
            .push_back(
                p[0]);
    }

    vector<bool>
        vis(
            numCourses);

    vector<bool>
        path(
            numCourses);

    for(int i=0;
        i<numCourses;
        i++){

        if(!vis[i]){

            if(
                dfs(
                    i,
                    vis,
                    path,
                    adj))

                return false;
        }
    }

    return true;
}
```

---

# Why Traverse All Nodes?

Graph may be:

```txt id="4aigms"
disconnected
```

Need to check:

- every component.

---

# Complexity

Adjacency list:

O(V+E)

Space:

O(V)

Optimal.

---

# Important Insight

Cycle in directed graph means:

```txt id="f4tqsb"
reaching a node
already present
in current recursion chain
```

That is exactly what:

```cpp id="l5q2vy"
path[]
```

stores.

---

# Common Mistakes

❌ Using only `vis[]`.

---

❌ Using parent logic (works only in undirected graph).

---

❌ Forgetting:

```cpp id="ngyyx7"
path[node]=false
```

after DFS finishes.

---

❌ Returning:

```cpp id="u5xqpn"
false
```

instead of:

```cpp id="b45h2m"
true
```

when cycle detected.

---

# Pattern Recognition

Classic pattern:

```txt id="t6jv4l"
DFS
+
recursion stack
+
cycle detection
```

Used in:

- Course Schedule
- Eventual Safe States
- Topological Sort
- Dependency Graphs
- Directed Graph Problems
