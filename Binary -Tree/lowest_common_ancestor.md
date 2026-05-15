Link : [LeetCode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/description/)

---

# Lowest Common Ancestor in Binary Tree

## Key Idea

For every node:

- check left subtree
- check right subtree

If:

- one node found on left
- one node found on right

→ current node = LCA

---

# Recursive Meaning

```cpp
dfs(root)
```

returns:

- `p` or `q` if found
- LCA if determined
- `nullptr` otherwise

---

# Cases

| left | right | return     |
| ---- | ----- | ---------- |
| null | null  | null       |
| node | null  | node       |
| null | node  | node       |
| node | node  | root (LCA) |

---

# Base Cases

```cpp
if(root == nullptr)
    return nullptr;

if(root == p || root == q)
    return root;
```

---

# Template

```cpp
TreeNode* dfs(TreeNode* root, TreeNode* p, TreeNode* q){

    if(root == nullptr || root == p || root == q)
        return root;

    TreeNode* left = dfs(root->left, p, q);
    TreeNode* right = dfs(root->right, p, q);

    if(left && right)
        return root;

    return left ? left : right;
}
```

---

# Complexity

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n)       |
| Space  | O(h)       |

- `h` = tree height

---

# Important Observation

If both recursive calls return non-null:

- current node is first split point
- therefore LCA

---

# Common Mistakes

❌ Comparing values

```cpp
root->val == p->val
```

✅ Compare pointers

```cpp
root == p
```

---

❌ Using global variables unnecessarily

Prefer recursive return values.

---

# Pattern Recognition

This problem teaches:

- tree recursion
- divide & conquer
- subtree information propagation

Same pattern used in:

- Diameter of tree
- Balanced binary tree
- Tree DP
- Path problems
