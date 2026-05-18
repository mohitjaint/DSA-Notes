# Recover Binary Search Tree

Problem : [LeetCode - Recover Binary Search Tree](https://leetcode.com/problems/recover-binary-search-tree/description/?utm_source=chatgpt.com)

---

# Problem Idea

Two nodes in BST are swapped by mistake.

Need to:

- restore BST
- without changing tree structure.

Only swap node values back.

---

# Key BST Property

BST inorder traversal should be:

```txt id="m8q2p5"
strictly increasing
```

If nodes are swapped:

- inorder sequence contains inversions.

---

# Core Insight

During inorder traversal:

```cpp id="x5m1q7"
prev->val > root->val
```

indicates:

- BST violation/inversion.

---

# Two Possible Cases

---

# Case 1 — Adjacent Nodes Swapped

Example inorder:

```txt id="u4m7q1"
1 3 2 4
```

Only one inversion:

```txt id="k5m1q8"
3 > 2
```

Need to swap:

- `3`
- `2`

---

# Case 2 — Non-Adjacent Nodes Swapped

Example inorder:

```txt id="r8p2m5"
1 5 3 4 2 6
```

Two inversions:

- `5 > 3`
- `4 > 2`

Need to swap:

- first larger node (`5`)
- last smaller node (`2`)

---

# Variables Meaning

## `prev`

Previous inorder node.

Used for inversion detection.

---

## `first`

First larger misplaced node.

```cpp id="n4m7q2"
first = prev
```

---

## `second`

Smaller node from first inversion.

Useful for adjacent swap case.

```cpp id="g2m8q4"
second = root
```

---

## `next`

Smaller node from second inversion.

Useful for non-adjacent swap case.

```cpp id="m5q1p7"
next = root
```

---

# Inorder Logic

```cpp id="x4m7q2"
if(prev &&
   prev->val > root->val){

    if(!first){

        first = prev;
        second = root;
    }
    else{

        next = root;
    }
}
```

---

# Final Swap

## Non-Adjacent Swap

```cpp id="k7q1m5"
swap(first->val,
     next->val);
```

---

## Adjacent Swap

```cpp id="u5m8q2"
swap(first->val,
     second->val);
```

---

# Complete Template

```cpp id="x2q8m4"
class Solution {
public:

    TreeNode* prev = nullptr;

    TreeNode* first = nullptr;

    TreeNode* second = nullptr;

    TreeNode* next = nullptr;

    void inorder(TreeNode* root){

        if(!root) return;

        inorder(root->left);

        if(prev &&
           prev->val > root->val){

            if(!first){

                first = prev;
                second = root;
            }
            else{

                next = root;
            }
        }

        prev = root;

        inorder(root->right);
    }

    void recoverTree(TreeNode* root) {

        inorder(root);

        if(next)
            swap(first->val,
                 next->val);

        else
            swap(first->val,
                 second->val);
    }
};
```

---

# Cleaner Standard Optimization

Can simplify:

- `second`
- `next`

into single variable:

```txt id="w4m7q1"
second
```

because second keeps updating automatically.

---

# Standard Cleaner Version

```cpp id="m8q2p1"
if(prev &&
   prev->val > root->val){

    if(!first)
        first = prev;

    second = root;
}
```

At end:

```cpp id="m8q2p5"
swap(first->val,
     second->val);
```

---

# Complexity

| Metric | Complexity     |
| ------ | -------------- |
| Time   | O(n)           |
| Space  | O(h) recursion |

where:

```txt id="x5m1q7"
h = tree height
```

---

# Important Insight

Recover BST is fundamentally:

```txt id="u4m7q1"
inversion detection in inorder traversal
```

because inorder traversal of BST must remain sorted.

---

# Common Mistakes

❌ Returning early after first inversion.

Need full inorder traversal.

---

❌ Forgetting:

- adjacent swap
- non-adjacent swap
  both cases.

---

❌ Comparing wrong nodes.

Need:

```cpp id="k5m1q8"
prev->val > root->val
```

---

# Pattern Recognition

Very important BST pattern:

```txt id="r8p2m5"
BST inorder = sorted sequence
```

Many BST problems reduce to:

- sorted order reasoning
- inversion detection
- order violations.
