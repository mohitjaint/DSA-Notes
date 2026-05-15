# Construct Binary Tree from Preorder and Inorder Traversal

## Key Observations

### Preorder Traversal

```txt id="t7yqot"
Root appears first
```

Structure:

```txt id="4kwq9p"
[root][left subtree][right subtree]
```

---

### Inorder Traversal

```txt id="e3g59y"
Root splits left and right subtree
```

Structure:

```txt id="m9db0d"
[left subtree][root][right subtree]
```

---

# Main Idea

1. First preorder element = root
2. Find root index in inorder
3. Left part of inorder = left subtree
4. Right part of inorder = right subtree
5. Recursively build both subtrees

---

# Recursive Meaning

```cpp id="mv1w6o"
dfs(preL, preR, inL, inR)
```

means:

```txt id="gnrlsk"
Construct subtree using:

preorder[preL : preR)
inorder[inL : inR)
```

(half-open ranges)

---

# Important Formula

Suppose:

```txt id="e57n1h"
idx = root index in inorder
```

Then:

```txt id="0r9u2q"
leftSize = idx - inL
```

---

# Left Subtree

## Preorder Range

```txt id="nyp9x4"
preL+1 → preL+1+leftSize
```

---

## Inorder Range

```txt id="jlwmv1"
inL → idx
```

---

# Right Subtree

## Preorder Range

```txt id="jgb20n"
preL+1+leftSize → preR
```

---

## Inorder Range

```txt id="lhr9qm"
idx+1 → inR
```

---

# Base Case

```cpp id="brldz9"
if(preL >= preR || inL >= inR)
    return nullptr;
```

---

# Hashmap Optimization

Store:

```cpp id="o8rjiz"
unordered_map<int,int> pos;
```

mapping:

```txt id="jlwmf2"
value -> inorder index
```

to avoid repeated searching.

---

# Optimal Template

```cpp id="0jlwmx"
TreeNode* dfs(preL, preR, inL, inR){

    if(invalid range)
        return nullptr;

    root = preorder[preL]

    find root index in inorder

    compute left subtree size

    recursively build:
        left subtree
        right subtree

    return root;
}
```

---

# Complexity

## Without Hashmap

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n²)      |
| Space  | O(n)       |

---

## With Hashmap (Optimal)

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n)       |
| Space  | O(n)       |

---

# Common Mistakes

❌ Forgetting to create node

```cpp id="jlwmr9"
new TreeNode(...)
```

---

❌ Wrong preorder ranges for right subtree

---

❌ Mixing inclusive/exclusive ranges

Choose one style consistently.

---

❌ Linear searching inorder every recursion

Use hashmap.

---

# Pattern Recognition

This is a classic:

```txt id="x2kxj3"
Tree Construction + Recursive Range Splitting
```

problem.
