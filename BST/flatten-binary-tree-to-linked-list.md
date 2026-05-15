# Flatten Binary Tree to Linked List

Problem from [TakeUForward Tutorial](https://takeuforward.org/data-structure/flatten-binary-tree-to-linked-list?utm_source=chatgpt.com)

---

# Goal

Flatten binary tree into linked list in-place.

Final structure:

```txt id="v7m2q5"
root
  \
   preorder traversal chain
```

Rules:

- use right pointers only
- left pointers become nullptr.

---

# Final Order Required

```txt id="k4x8m1"
Preorder Traversal
```

Meaning:

```txt id="u1q7m4"
root → left → right
```

---

# Approach 1 — Recursive (Reverse Preorder)

## Core Idea

Build linked list backwards using:

```txt id="m8q2p5"
right → left → root
```

Maintain:

```txt id="x5m1q7"
prev node
```

---

# Logic

For every node:

```cpp id="g2m8q4"
root->right = prev;
root->left = nullptr;
prev = root;
```

---

# Why Reverse Preorder?

Desired:

```txt id="r7p1m5"
root → left → right
```

So build from back:

```txt id="n4x8q2"
right → left → root
```

---

# Code Template

```cpp id="w3m7q1"
TreeNode* prev = nullptr;

void flatten(TreeNode* root){

    if(!root)
        return;

    flatten(root->right);

    flatten(root->left);

    root->right = prev;

    root->left = nullptr;

    prev = root;
}
```

---

# Complexity

| Metric | Complexity     |
| ------ | -------------- |
| Time   | O(n)           |
| Space  | O(h) recursion |

---

# Approach 2 — Stack Based Iterative

## Core Idea

Simulate preorder traversal using stack.

---

# Logic

1. Push root
2. Pop node
3. Push right child
4. Push left child
5. Connect current node to next stack top

---

# Code Template

```cpp id="u8m2q5"
stack<TreeNode*> st;

st.push(root);

while(!st.empty()){

    TreeNode* curr = st.top();
    st.pop();

    if(curr->right)
        st.push(curr->right);

    if(curr->left)
        st.push(curr->left);

    if(!st.empty())
        curr->right = st.top();

    curr->left = nullptr;
}
```

---

# Complexity

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n)       |
| Space  | O(n)       |

---

# Approach 3 — Morris Traversal (Optimal)

From [TakeUForward Tutorial](https://takeuforward.org/data-structure/flatten-binary-tree-to-linked-list?utm_source=chatgpt.com)

---

# Core Idea

For every node:

1. find predecessor in left subtree
2. connect predecessor’s right to current right subtree
3. move left subtree to right
4. set left = nullptr

---

# Logic

Suppose:

```txt id="k5m1q8"
curr
```

has left subtree.

Find:

```txt id="x2p7m4"
rightmost node in left subtree
```

Then:

```cpp id="q8m2x5"
prev->right = curr->right;

curr->right = curr->left;

curr->left = nullptr;
```

Move:

```cpp id="t4m7q1"
curr = curr->right;
```

---

# Code Template

```cpp id="r9x2m5"
TreeNode* curr = root;

while(curr){

    if(curr->left){

        TreeNode* prev = curr->left;

        while(prev->right){

            prev = prev->right;
        }

        prev->right = curr->right;

        curr->right = curr->left;

        curr->left = nullptr;
    }

    curr = curr->right;
}
```

---

# Complexity

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n)       |
| Space  | O(1)       |

Optimal approach.

---

# Your Approach — Tail Returning Recursion

## Core Idea

Flatten subtree and return:

```txt id="m6q1p7"
tail node of flattened subtree
```

This avoids repeated traversal to tail.

---

# Recursive Meaning

```cpp id="v3m8q2"
dfs(root)
```

returns:

```txt id="u7x1m4"
tail of flattened subtree
```

---

# Logic

1. Save left/right subtree
2. Attach left subtree to right
3. Flatten left subtree
4. Attach original right subtree at tail
5. Flatten right subtree
6. Return final tail

---

# Your Optimized Version

```cpp id="p8m2q5"
TreeNode* dfs(TreeNode* root){

    if(!root)
        return nullptr;

    TreeNode* left = root->left;
    TreeNode* right = root->right;

    TreeNode* tail = root;

    if(left){

        root->right = left;
        root->left = nullptr;

        tail = dfs(left);
    }

    if(right){

        tail->right = right;

        tail = dfs(right);
    }

    return tail;
}
```

---

# Complexity

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n)       |
| Space  | O(h)       |

---

# Important Insight

Returning extra information from recursion is a powerful pattern.

Examples:

- subtree height
- subtree diameter
- subtree sum
- subtree tail

Your solution uses:

```txt id="y4m7q2"
tail-return recursion
```

which is a valid and elegant approach.

---

# Common Mistakes

❌ Repeatedly traversing to tail

Can become:

O(n^2)

---

❌ Forgetting:

```cpp id="w8m2q4"
root->left = nullptr;
```

---

❌ Losing original right subtree before reconnecting.

---

# Pattern Recognition

This problem combines:

- preorder traversal
- tree restructuring
- pointer manipulation
- recursive design.
