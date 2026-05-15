# Kth Smallest Element in BST

## Key BST Property

For BST:

```txt id="m8q2p5"
left subtree < root < right subtree
```

So:

```txt id="x5m1q7"
inorder traversal gives sorted order
```

Traversal order:

```txt id="u4m7q1"
left → root → right
```

---

# Main Idea

Perform inorder traversal.

Maintain:

- current position count
- answer when count becomes `k`.

---

# Logic

## Steps

1. Traverse left subtree
2. Visit current node
3. Increment counter
4. If counter == k → answer found
5. Traverse right subtree

---

# Correct Recursive Template

```cpp id="k5m1q8"
void inorder(TreeNode* root,
             int k,
             int& pos,
             int& ans){

    if(!root)
        return;

    inorder(root->left,
            k,
            pos,
            ans);

    pos++;

    if(pos == k){

        ans = root->val;
        return;
    }

    inorder(root->right,
            k,
            pos,
            ans);
}
```

---

# Main Function

```cpp id="r8p2m5"
int kthSmallest(TreeNode* root, int k){

    int pos = 0;
    int ans = -1;

    inorder(root, k, pos, ans);

    return ans;
}
```

---

# Why Inorder Works

Inorder traversal of BST produces:

```txt id="n4m7q2"
sorted sequence
```

Example:

```txt id="g2m8q4"
      5
     / \
    3   7
```

Inorder:

```txt id="m5q1p7"
3 5 7
```

Sorted.

---

# Complexity

| Metric | Complexity                |
| ------ | ------------------------- |
| Time   | O(k) average / O(n) worst |
| Space  | O(h) recursion            |

where:

```txt id="x4m7q2"
h = tree height
```

---

# Follow-Up Optimal Space Solution

Can use:

```txt id="k7q1m5"
Morris Inorder Traversal
```

to achieve:

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n)       |
| Space  | O(1)       |

---

# Common Mistakes

❌ Incrementing counter for null nodes

---

❌ Using preorder/postorder traversal

Need inorder traversal.

---

❌ Forgetting BST property

This solution works because tree is BST.

---

# Important Insight

BST + inorder traversal ⇒

```txt id="u5m8q2"
sorted order without extra sorting
```

Very important BST pattern.

---

# Pattern Recognition

Questions involving:

- kth smallest
- kth largest
- sorted BST output

usually suggest:

```txt id="x2q8m4"
inorder traversal
```
