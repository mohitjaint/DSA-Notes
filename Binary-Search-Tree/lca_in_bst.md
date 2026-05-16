# Lowest Common Ancestor in BST

Link : [LeetCode Problem](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/?utm_source=chatgpt.com)

---

# Key BST Property

```txt id="m8q2p5"
left subtree < root < right subtree
```

BST ordering allows us to determine traversal direction immediately.

---

# Core Idea

## Both Nodes Smaller

If:

```cpp id="x5m1q7"
root->val > p->val &&
root->val > q->val
```

then both nodes lie in left subtree.

→ go left.

---

## Both Nodes Larger

If:

```cpp id="u4m7q1"
root->val < p->val &&
root->val < q->val
```

then both nodes lie in right subtree.

→ go right.

---

## Otherwise

Current node is:

- split point
  OR
- equal to one of the nodes.

Therefore:

```txt id="k5m1q8"
current node = LCA
```

---

# Important Observation

If:

- one node lies on left
- one node lies on right

then current node is the first common ancestor.

---

# Recursive Template

```cpp id="r8p2m5"
TreeNode* lowestCommonAncestor(
    TreeNode* root,
    TreeNode* p,
    TreeNode* q){

    if(root->val > p->val &&
       root->val > q->val)

        return lowestCommonAncestor(
            root->left, p, q);

    if(root->val < p->val &&
       root->val < q->val)

        return lowestCommonAncestor(
            root->right, p, q);

    return root;
}
```

---

# Iterative Version

```cpp id="n4m7q2"
while(root){

    if(root->val > p->val &&
       root->val > q->val)

        root = root->left;

    else if(root->val < p->val &&
            root->val < q->val)

        root = root->right;

    else
        return root;
}
```

---

# Complexity

| Metric | Complexity     |
| ------ | -------------- |
| Time   | O(h)           |
| Space  | O(h) recursion |

Iterative version:

| Space | O(1) |

where:

```txt id="g2m8q4"
h = height of BST
```

---

# Difference from Normal Binary Tree LCA

| Binary Tree          | BST                |
| -------------------- | ------------------ |
| search both subtrees | only one direction |
| O(n)                 | O(h)               |

BST ordering greatly simplifies problem.

---

# Common Mistakes

❌ Assuming:

- `p < q`

Cleaner solution avoids ordering assumptions.

---

❌ Searching both subtrees unnecessarily.

BST property already determines direction.

---

# Pattern Recognition

BST problems often reduce to:

```txt id="m5q1p7"
using ordering property
to eliminate half search space
```
