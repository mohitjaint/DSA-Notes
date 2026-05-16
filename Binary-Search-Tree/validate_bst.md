# Validate BST — Concise Notes

## Key Idea

BST condition is:

```txt id="m8q2p5"
low < node < high
```

for every node.

Need global subtree constraints, not just parent-child comparison.

---

# Recursive Meaning

```cpp id="x5m1q7"
dfs(root, low, high)
```

checks whether subtree values lie within allowed range.

---

# Logic

## Invalid Condition

```cpp id="u4m7q1"
if(root->val <= low ||
   root->val >= high)
```

return false.

---

# Recursive Calls

## Left Subtree

```cpp id="k5m1q8"
dfs(root->left, low, root->val)
```

---

## Right Subtree

```cpp id="r8p2m5"
dfs(root->right, root->val, high)
```

---

# Template

```cpp id="n4m7q2"
bool dfs(TreeNode* root,
         long long low,
         long long high){

    if(!root)
        return true;

    if(root->val <= low ||
       root->val >= high)
        return false;

    return dfs(root->left,
               low,
               root->val)
        &&
           dfs(root->right,
               root->val,
               high);
}
```

---

# Important Detail

Use:

```cpp id="g2m8q4"
LLONG_MIN
LLONG_MAX
```

to avoid overflow issues.

---

# Complexity

| Time  | O(n)           |
| ----- | -------------- |
| Space | O(h) recursion |

---

# Common Mistake

❌ Checking only:

```txt id="m5q1p7"
left < root < right
```

locally.

Need subtree-wide bounds.
