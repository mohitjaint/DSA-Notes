Link : [geeksforgeek](https://www.geeksforgeeks.org/problems/children-sum-parent/1)

---

# Children Sum Property

## Property

For every node:

```txt
node->val = left->val + right->val
```

Null child contributes `0`.

Leaf nodes always satisfy property.

---

# Recursive Idea

For every node:

1. validate current node
2. recursively validate left/right subtrees

---

# Base Case

```cpp
if(!root || (!root->left && !root->right))
    return true;
```

- null node → valid
- leaf node → valid

---

# Sum Trick

Instead of handling:

- both children
- only left
- only right

separately,

use:

```cpp
int sum = 0;

if(root->left)
    sum += root->left->val;

if(root->right)
    sum += root->right->val;
```

Then:

```cpp
root->val == sum
```

Much cleaner.

---

# Template

```cpp
bool dfs(TreeNode* root){

    if(!root || (!root->left && !root->right))
        return true;

    int sum = 0;

    if(root->left)
        sum += root->left->val;

    if(root->right)
        sum += root->right->val;

    return (root->val == sum)
        && dfs(root->left)
        && dfs(root->right);
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

# Common Mistakes

❌ Forgetting single child case

Null child contributes `0`.

---

❌ Overcomplicated branching

Prefer generalized sum approach.

---

❌ Forgetting leaf nodes are always valid

```cpp
if(leaf) return true;
```
