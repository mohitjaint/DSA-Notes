# Maximum Sum BST in Binary Tree

Problem : [LeetCode - Maximum Sum BST in Binary Tree](https://leetcode.com/problems/maximum-sum-bst-in-binary-tree/description/?utm_source=chatgpt.com)

---

# Problem Idea

Given a binary tree.

Need to find:

```txt
maximum sum among all BST subtrees
```

Subtree:

- can be any subtree
- not necessarily entire tree.

---

# Key Observation

To determine whether current subtree is BST:

Parent needs information from children.

Each subtree should return:

```txt
(sum, maximum value, minimum value)
```

---

# Why Postorder?

Current node depends on:

```txt
left subtree
right subtree
```

So process:

```txt
left → right → root
```

Postorder traversal.

---

# Recursive Meaning

```cpp
dfs(root)
```

returns:

| Value | Meaning                  |
| ----- | ------------------------ |
| sum   | subtree sum              |
| mx    | maximum value in subtree |
| mn    | minimum value in subtree |

---

# BST Validation Condition

Current subtree is BST only if:

```cpp
root->val > left.max
```

AND

```cpp
root->val < right.min
```

---

# If Current Subtree Is BST

Compute:

```cpp
sum =
left.sum +
right.sum +
root->val
```

Update answer.

Return:

```txt
(
sum,
max(root, right.max),
min(root, left.min)
)
```

---

# If Current Subtree Is NOT BST

Return invalid range:

```cpp
{
0,
INT_MAX,
INT_MIN
}
```

This guarantees parent BST check fails.

---

# Cleaner Tuple Solution

```cpp
class Solution {
public:

    int ans = 0;

    tuple<int,int,int> dfs(TreeNode* root){

        if(!root)
            return {0, INT_MIN, INT_MAX};

        auto [ls,lmax,lmin]
            = dfs(root->left);

        auto [rs,rmax,rmin]
            = dfs(root->right);

        if(root->val > lmax &&
           root->val < rmin){

            int sum =
                ls + rs + root->val;

            ans =
                max(ans, sum);

            return {
                sum,
                max(root->val, rmax),
                min(root->val, lmin)
            };
        }

        return {
            0,
            INT_MAX,
            INT_MIN
        };
    }

    int maxSumBST(TreeNode* root){

        dfs(root);

        return ans;
    }
};
```

---

# Cleaner Alternative — Struct

Instead of:

```cpp
get<0>()
get<1>()
get<2>()
```

use:

```cpp
l.sum
l.mx
l.mn
```

Usually easier to read.

---

# Complexity

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n)       |
| Space  | O(h)       |

where:

```txt
h = tree height
```

---

# Important Insight

This problem teaches:

```txt
subtree information propagation
```

Each subtree returns enough information so parent can decide:

```txt
Am I BST?
What is my sum?
What is my range?
```

---

# Common Mistakes

❌ Using preorder/inorder.

Need postorder.

---

❌ Returning wrong:

```txt
min / max
```

values.

---

❌ Returning subtree sum as final answer.

Need:

```txt
global maximum
```

because best BST may be deep inside tree.

---

❌ Returning invalid range incorrectly.

Correct invalid return:

```cpp
{
0,
INT_MAX,
INT_MIN
}
```

---

# Pattern Recognition

Very important Tree DP pattern:

```txt
child → parent information flow
```

Same idea appears in:

- Largest BST
- Balanced Binary Tree
- Diameter
- Tree DP
- Maximum Path Sum
- Validate BST recursively
