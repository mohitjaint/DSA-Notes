# Lowest Common Ancestor of Binary Tree

Link : [LeetCode Problem](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/description/?utm_source=chatgpt.com)

---

# Core Idea

For every node:

- search left subtree
- search right subtree

If:

- one target found in left subtree
- other target found in right subtree

then:

```txt id="m8q2p5"
current node = Lowest Common Ancestor
```

---

# Recursive Meaning

```cpp id="x5m1q7"
dfs(root)
```

returns:

- `p` if subtree contains `p`
- `q` if subtree contains `q`
- `LCA` if already found
- `nullptr` if neither exists

---

# Key Observation

If both recursive calls are non-null:

```cpp id="u4m7q1"
left != nullptr
right != nullptr
```

then current node is the:

- first common split point
- therefore LCA.

---

# Base Cases

```cpp id="k5m1q8"
if(root == nullptr)
    return nullptr;

if(root == p || root == q)
    return root;
```

Why?

If current node itself is:

- `p`
  OR
- `q`

return it upward immediately.

---

# Recursive Logic

```cpp id="r8p2m5"
TreeNode* left =
    dfs(root->left, p, q);

TreeNode* right =
    dfs(root->right, p, q);
```

---

# Cases

| left | right | Meaning                | Return |
| ---- | ----- | ---------------------- | ------ |
| null | null  | neither found          | null   |
| node | null  | found in left subtree  | left   |
| null | node  | found in right subtree | right  |
| node | node  | split point found      | root   |

---

# Final Template

```cpp id="n4m7q2"
TreeNode* dfs(TreeNode* root,
              TreeNode* p,
              TreeNode* q){

    if(root == nullptr ||
       root == p ||
       root == q)
        return root;

    TreeNode* left =
        dfs(root->left, p, q);

    TreeNode* right =
        dfs(root->right, p, q);

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

where:

- `n` = number of nodes
- `h` = tree height.

---

# Important Insight

This is a classic:

```txt id="g2m8q4"
bottom-up tree recursion
```

problem.

Each subtree returns information upward to parent.

---

# Common Mistakes

## ❌ Comparing values

```cpp id="m5q1p7"
root->val == p->val
```

Wrong if duplicate values exist.

✅ Correct:

```cpp id="x4m7q2"
root == p
```

(compare pointers)

---

## ❌ Using global variables unnecessarily

Recursive return values already contain required information.

---

## ❌ Overcomplicating with paths

Path storing works but is unnecessary.

---

# Pattern Recognition

This pattern appears in:

- Tree DP
- Diameter of tree
- Balanced binary tree
- Path sum problems
- Subtree information propagation

Core theme:

```txt id="k7q1m5"
collect information from children
and combine at parent
```
