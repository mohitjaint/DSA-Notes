# Count Complete Tree Nodes

## Key Observation

For a complete binary tree:

If:

```txt id="t6s57s"
left height == right height
```

then subtree is a perfect binary tree.

Number of nodes directly:

2^h - 1

No need to recurse further.

---

# Left Height

Move continuously left.

```cpp id="jlwm9g"
while(root){

    h++;
    root = root->left;
}
```

---

# Right Height

Move continuously right.

```cpp id="vxaj3f"
while(root){

    h++;
    root = root->right;
}
```

---

# Cases

## Perfect Binary Tree

```cpp id="58n0zv"
if(lh == rh)
    return (1LL << lh) - 1;
```

---

## Incomplete Tree

Recursively count:

```cpp id="zwk3vv"
1 + left subtree + right subtree
```

---

# Template

```cpp id="4n0c1e"
int countNodes(TreeNode* root){

    if(!root)
        return 0;

    int lh = leftHeight(root);
    int rh = rightHeight(root);

    if(lh == rh)
        return (1LL << lh) - 1;

    return 1
         + countNodes(root->left)
         + countNodes(root->right);
}
```

---

# Complexity

| Metric | Complexity   |
| ------ | ------------ |
| Time   | O((log n)^2) |
| Space  | O(log n)     |

---

# Why Complexity is Better

Normal traversal:

```txt id="vnlh33"
O(n)
```

This solution:

- skips perfect subtrees directly
- avoids visiting every node.

---

# Important Restriction

Optimization works only for:

```txt id="7d1r0z"
Complete Binary Tree
```

NOT arbitrary binary trees.

---

# Common Mistakes

❌ Using on normal binary tree

---

❌ Overflow in:

```cpp id="6gkl3u"
1 << h
```

Use:

```cpp id="rgwb7g"
1LL << h
```

---

# Pattern Recognition

This problem teaches:

```txt id="ah4jbm"
Use special tree properties
to avoid full traversal
```
