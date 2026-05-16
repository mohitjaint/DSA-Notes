# Number of Provinces — DSU / Union Find

Problem : [LeetCode - Number of Provinces](https://leetcode.com/problems/number-of-provinces/description/?utm_source=chatgpt.com)

---

# Core Idea

Each city initially belongs to its own component.

If:

```txt id="m8q2p5"
isConnected[i][j] == 1
```

then:

- both cities belong to same province
- merge their components.

Final answer:

```txt id="x5m1q7"
number of unique connected components
```

---

# DSU Operations

## Find

Returns:

```txt id="u4m7q1"
representative/root of component
```

---

## Union

Merges:

- two different components.

---

# Path Compression

```cpp id="k5m1q8"
return parent[x] =
       find(parent, parent[x]);
```

Flattens DSU tree.

Optimizes future operations.

---

# Find Function

```cpp id="r8p2m5"
int find(vector<int>& parent, int x){

    if(parent[x] == x)
        return x;

    return parent[x] =
           find(parent, parent[x]);
}
```

---

# Union Logic

```cpp id="n4m7q2"
int pu = find(parent, i);
int pv = find(parent, j);

if(pu != pv)
    parent[pu] = pv;
```

Always merge:

- roots
  NOT
- arbitrary nodes.

---

# Matrix Optimization

Adjacency matrix is symmetric:

```txt id="g2m8q4"
isConnected[i][j]
=
isConnected[j][i]
```

So traverse only upper triangle:

```cpp id="m5q1p7"
for(int j = i + 1; j < n; j++)
```

Avoids duplicate unions.

---

# Counting Provinces

Find unique roots:

```cpp id="x4m7q2"
for(int i = 0; i < n; i++){

    st.insert(find(parent, i));
}
```

Size of set = number of provinces.

---

# Complete Template

```cpp id="k7q1m5"
class Solution {
public:

    int find(vector<int>& parent, int x){

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

            for(int j = i + 1; j < n; j++){

                if(isConnected[i][j]){

                    int pu = find(parent, i);
                    int pv = find(parent, j);

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

# Complexity

| Metric | Complexity   |
| ------ | ------------ |
| Time   | O(n² · α(n)) |
| Space  | O(n)         |

where:

```txt id="u5m8q2"
α(n)
```

is inverse Ackermann function (almost constant).

---

# Important Insight

DSU is used for:

```txt id="x2q8m4"
dynamic connected components
```

Very important graph pattern.

---

# Common Mistakes

❌ Unioning arbitrary nodes instead of roots.

---

❌ Forgetting path compression.

---

❌ Traversing full symmetric matrix unnecessarily.

---

# Pattern Recognition

DSU is commonly used in:

- connected components
- cycle detection
- Kruskal’s algorithm
- dynamic connectivity
- grouping/merging problems.
