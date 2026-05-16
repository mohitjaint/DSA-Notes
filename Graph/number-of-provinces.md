# Number of Provinces

Problem : [LeetCode - Number of Provinces](https://leetcode.com/problems/number-of-provinces/description/?utm_source=chatgpt.com)

---

# Problem Idea

Given:

- adjacency matrix of an undirected graph

Need to find:

```txt id="m8q2p5"
number of connected components
```

Each connected component = one province.

---

# Approach 1 — DFS

---

# Core Idea

Start DFS from every unvisited node.

Each DFS traversal visits:

- one complete connected component/province.

---

# DFS Logic

```cpp id="x5m1q7"
dfs(node)
```

marks:

- all reachable nodes as visited.

---

# DFS Template

```cpp id="u4m7q1"
class Solution {
public:

    void dfs(vector<vector<int>>& m,
             int node,
             vector<int>& vis){

        vis[node] = 1;

        for(int i = 0; i < m.size(); i++){

            if(m[node][i] == 1 &&
               !vis[i]){

                dfs(m, i, vis);
            }
        }
    }

    int findCircleNum(
        vector<vector<int>>& isConnected) {

        int n = isConnected.size();

        vector<int> vis(n);

        int cnt = 0;

        for(int i = 0; i < n; i++){

            if(!vis[i]){

                dfs(isConnected,
                    i,
                    vis);

                cnt++;
            }
        }

        return cnt;
    }
};
```

---

# DFS Complexity

Adjacency matrix traversal:

| Metric | Complexity               |
| ------ | ------------------------ |
| Time   | O(n²)                    |
| Space  | O(n) recursion + visited |

---

# DFS Use Cases

Best when:

- graph is static
- need traversal
- need connected components
- need path exploration.

Examples:

- Number of Provinces
- Number of Islands
- Flood Fill
- Graph Traversal
- Cycle Detection.

---

# Approach 2 — DSU / Union Find

---

# Core Idea

Initially:

- every node belongs to separate component.

If edge exists:

- merge both components.

Final answer:

- number of unique component roots.

---

# DSU Operations

## Find

Returns:

```txt id="k5m1q8"
representative/root of component
```

---

## Union

Merges:

- two different components.

---

# Path Compression

```cpp id="r8p2m5"
parent[x] =
    find(parent, parent[x]);
```

Flattens DSU tree.

Optimizes future operations.

---

# DSU Template

```cpp id="n4m7q2"
class Solution {
public:

    int find(vector<int>& parent,
             int x){

        if(parent[x] == x)
            return x;

        return parent[x] =
               find(parent, parent[x]);
    }

    int findCircleNum(
        vector<vector<int>>& isConnected) {

        int n = isConnected.size();

        vector<int> parent(n);

        for(int i = 0; i < n; i++)
            parent[i] = i;

        for(int i = 0; i < n; i++){

            for(int j = i + 1;
                j < n;
                j++){

                if(isConnected[i][j]){

                    int pu =
                        find(parent, i);

                    int pv =
                        find(parent, j);

                    if(pu != pv)
                        parent[pu] = pv;
                }
            }
        }

        unordered_set<int> st;

        for(int i = 0; i < n; i++){

            st.insert(find(parent, i));
        }

        return st.size();
    }
};
```

---

# DSU Complexity

| Metric | Complexity   |
| ------ | ------------ |
| Time   | O(n² · α(n)) |
| Space  | O(n)         |

where:

```txt id="g2m8q4"
α(n)
```

is inverse Ackermann function (almost constant).

Since input itself is adjacency matrix:

O(n^2)

is unavoidable.

---

# DFS vs DSU Comparison

| Feature              | DFS               | DSU              |
| -------------------- | ----------------- | ---------------- |
| Idea                 | Explore component | Merge components |
| Traversal Needed     | Yes               | No               |
| Dynamic Connectivity | Poor              | Excellent        |
| Simpler              | ✅                | Slightly harder  |
| Static Graphs        | Excellent         | Good             |
| Online Queries       | Poor              | Excellent        |

---

# When DFS Is Better

Use DFS/BFS when:

- graph is static
- need traversal/path exploration
- need component counting once.

Examples:

- Flood fill
- Number of islands
- Province counting
- Maze traversal.

---

# When DSU Is Better

Use DSU when:

- edges added dynamically
- repeated connectivity queries
- frequent component merging.

Examples:

- Kruskal MST
- Accounts Merge
- Number of Islands II
- Dynamic friend networks.

---

# Important Insight

## DFS Thinking

```txt id="m5q1p7"
“Explore connected region.”
```

---

## DSU Thinking

```txt id="x4m7q2"
“Maintain component structure.”
```

Both are fundamental graph paradigms.

---

# Common Mistakes

## DFS

❌ Forgetting visited array.

---

❌ Traversing incomplete neighbors.

---

## DSU

❌ Unioning arbitrary nodes instead of roots.

---

❌ Forgetting path compression.

---

❌ Not understanding representative/root concept.
