Link : [leetcode](https://leetcode.com/problems/maximum-width-of-binary-tree/)

---

# Maximum Width of Binary Tree

## Key Idea

Assign heap-style indices to nodes.

For node at index `i`:

```txt
left  = 2*i + 1
right = 2*i + 2
```

Width of a level:

```txt
last_index - first_index + 1
```

---

# Approach

Use BFS level order traversal.

Store:

```cpp
(node, index)
```

in queue.

---

# Important Observation

Even if nodes are missing between two nodes,
their virtual indices still count toward width.

---

# Overflow Handling

Indices can grow exponentially.

Normalize every level:

```cpp
idx -= first
```

This keeps numbers small.

Also use:

```cpp
long long
```

for indices.

---

# Template

```cpp
queue<pair<TreeNode*, long long>> q;

q.push({root, 0});

while(!q.empty()) {

    int size = q.size();

    long long first = q.front().second;
    long long last = q.back().second;

    ans = max(ans, last - first + 1);

    for(int i = 0; i < size; i++) {

        auto [node, idx] = q.front();
        q.pop();

        idx -= first;

        if(node->left)
            q.push({node->left, 2*idx + 1});

        if(node->right)
            q.push({node->right, 2*idx + 2});
    }
}
```

---

# Complexity

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n)       |
| Space  | O(n)       |

---

# Common Mistakes

❌ Using `int` for indices

Use:

```cpp
long long
```

---

❌ Pushing null nodes into queue

Store only valid nodes.

---

❌ Forgetting normalization

```cpp
idx -= first
```

needed to avoid overflow.

---

❌ Debug `cout` slowing solution

Avoid printing inside loops.
