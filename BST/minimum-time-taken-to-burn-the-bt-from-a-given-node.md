# Minimum Time to Burn Binary Tree

## Key Observation

Fire spreads in 3 directions:

- left child
- right child
- parent

So tree behaves like an undirected graph.

---

# Main Idea

1. Store parent of every node
2. Find target/start node
3. Perform BFS from target
4. Each BFS level = 1 second

---

# Parent Mapping

```cpp id="gn5y1n"
parent[child] = node;
```

Needed because tree normally has no parent pointer.

---

# BFS Intuition

At every second:

- currently burning nodes spread fire to neighbours.

This is exactly:

```txt id="g1fz0r"
level-order BFS
```

---

# BFS Pattern

```cpp id="pf9n0t"
while(!q.empty()){

    int size = q.size();

    process current level

    time++;
}
```

Each BFS level corresponds to:

- one unit time.

---

# Neighbours of Node

```cpp id="hkt1oe"
node->left
node->right
parent[node]
```

---

# Visited Set

Necessary to avoid revisiting:

```txt id="0b3c4n"
parent <-> child
```

cycles.

---

# BFS Template

```cpp id="yq14f6"
queue<TreeNode*> q;
unordered_set<TreeNode*> vis;

q.push(target);
vis.insert(target);

while(!q.empty()){

    int size = q.size();

    for(int i = 0; i < size; i++){

        TreeNode* node = q.front();
        q.pop();

        process neighbours
    }

    time++;
}
```

---

# Important Checks

Always verify node exists before pushing:

```cpp id="l4ev4l"
if(node && !vis.count(node))
```

---

# Complexity

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n)       |
| Space  | O(n)       |

---

# Common Mistakes

❌ Forgetting parent traversal

---

❌ Missing visited set

Causes infinite revisits.

---

❌ Pushing `nullptr` into queue

---

❌ Forgetting:

```txt id="xfz4ow"
One BFS level = one second
```

---

# Pattern Recognition

Very similar to:

- Nodes at distance K
- Graph BFS
- Multi-source BFS
- Infection spread problems

---

# Core Pattern

```txt id="5n4g0h"
Tree + parent links
=
Undirected graph
```
