# Binary Search Tree Iterator

Problem : [LeetCode - Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/description/?utm_source=chatgpt.com)

---

# Key BST Property

Inorder traversal of BST gives:

```txt id="m8q2p5"
sorted order
```

Iterator must return:

- next smallest element
- efficiently
- without storing full traversal.

---

# Core Idea

Use stack to simulate:

```txt id="x5m1q7"
iterative inorder traversal
```

Stack stores:

```txt id="u4m7q1"
path to next smallest node
```

---

# Constructor Logic

Initially push:

```txt id="k5m1q8"
entire left chain
```

because leftmost node is smallest.

```cpp id="r8p2m5"
while(root){

    st.push(root);

    root = root->left;
}
```

---

# next() Logic

Top of stack:

- current smallest node.

After popping:

- process its right subtree
- push left chain of right subtree.

---

# next() Template

```cpp id="n4m7q2"
TreeNode* node = st.top();

st.pop();

int val = node->val;

node = node->right;

while(node){

    st.push(node);

    node = node->left;
}

return val;
```

---

# hasNext() Logic

```cpp id="g2m8q4"
return !st.empty();
```

If stack non-empty:

- next inorder node exists.

---

# Why It Works

Stack always maintains:

- next inorder candidate.

Effectively simulates recursive inorder traversal lazily.

---

# Complexity

| Operation | Complexity     |
| --------- | -------------- |
| next()    | O(1) amortized |
| hasNext() | O(1)           |
| Space     | O(h)           |

where:

```txt id="m5q1p7"
h = tree height
```

---

# Why next() Is Amortized O(1)

Each node:

- pushed once
- popped once.

Total stack operations:

2n

over entire traversal.

So average per operation:

O(1)

---

# Better Than Brute Force

Brute force:

- store full inorder traversal.

| Space | O(n) |

Iterator approach:

| Space | O(h) |

Much more efficient.

---

# Important Insight

This problem teaches:

```txt id="x4m7q2"
lazy traversal
```

Generate values:

- only when requested.

---

# Common Mistakes

❌ Forgetting to process:

- left chain of right subtree.

---

❌ Storing entire inorder unnecessarily.

---

❌ Thinking next() is O(h) overall.

Amortized complexity is O(1).

---

# Pattern Recognition

Classic:

- stack simulation
- inorder traversal
- iterator design
- lazy evaluation pattern.
