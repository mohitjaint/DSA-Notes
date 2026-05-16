# Construct BST from Preorder Traversal

Link : [LeetCode Problem](https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/description/?utm_source=chatgpt.com)

---

# Key Observations

## BST Property

```txt id="m8q2p5"
left subtree < root < right subtree
```

---

## Preorder Traversal

```txt id="x5m1q7"
root → left → right
```

So:

- first element = root
- then left subtree values
- then right subtree values.

---

# Optimal Idea — Bounds

Instead of searching partition point repeatedly:

```txt id="u4m7q1"
carry allowed upper bound
```

during recursion.

---

# Recursive Meaning

```cpp id="k5m1q8"
build(preorder, bound)
```

builds BST subtree using preorder elements:

```txt id="r8p2m5"
< bound
```

---

# Core Insight

If:

```cpp id="n4m7q2"
preorder[idx] > bound
```

then current value does NOT belong to current subtree.

Return:

```cpp id="g2m8q4"
nullptr
```

---

# Logic

## Create Root

```cpp id="m5q1p7"
TreeNode* root =
    new TreeNode(preorder[idx++]);
```

---

## Build Left Subtree

Left subtree values must satisfy:

```txt id="x4m7q2"
value < root->val
```

So:

```cpp id="k7q1m5"
root->left =
    build(preorder, root->val);
```

---

## Build Right Subtree

Right subtree still follows parent bound.

```cpp id="u5m8q2"
root->right =
    build(preorder, bound);
```

---

# Optimal Template

```cpp id="x2q8m4"
class Solution {
public:

    int idx = 0;

    TreeNode* build(vector<int>& preorder,
                    int bound){

        if(idx == preorder.size() ||
           preorder[idx] > bound)

            return nullptr;

        TreeNode* root =
            new TreeNode(preorder[idx++]);

        root->left =
            build(preorder, root->val);

        root->right =
            build(preorder, bound);

        return root;
    }

    TreeNode* bstFromPreorder(
        vector<int>& preorder) {

        return build(preorder, INT_MAX);
    }
};
```

---

# Why It Works

Preorder already guarantees traversal order.

Bounds guarantee BST validity.

Each node is processed exactly once.

---

# Complexity

| Metric | Complexity     |
| ------ | -------------- |
| Time   | O(n)           |
| Space  | O(h) recursion |

where:

- `n` = number of nodes
- `h` = tree height.

---

# Brute Force Approach

Find first greater element manually:

- left part → left subtree
- right part → right subtree.

Complexity:
