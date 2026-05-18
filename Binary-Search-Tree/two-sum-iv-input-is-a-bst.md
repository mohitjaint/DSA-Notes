# Two Sum IV - Input is a BST

Problem : [LeetCode - Two Sum IV - Input is a BST](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/description/?utm_source=chatgpt.com)

---

# Problem Idea

Need to determine if there exist:

```txt id="m8q2p5"
two different nodes
```

such that:

a+b=k

---

# Key BST Property

BST inorder traversal gives:

```txt id="x5m1q7"
sorted order
```

This converts problem into:

- classic two sum on sorted sequence.

---

# Approach 1 — Inorder + Two Pointers

---

# Core Idea

## Step 1

Store inorder traversal in vector.

Since BST inorder is sorted:

```txt id="u4m7q1"
vector becomes sorted
```

---

## Step 2

Apply:

- classic two pointer technique.

---

# Inorder Traversal

```cpp id="k5m1q8"
void inorder(TreeNode* root,
             vector<int>& v){

    if(!root) return;

    inorder(root->left, v);

    v.push_back(root->val);

    inorder(root->right, v);
}
```

---

# Two Pointer Logic

```cpp id="r8p2m5"
int i = 0;
int j = v.size() - 1;

while(i < j){

    if(v[i] + v[j] == k)
        return true;

    else if(v[i] + v[j] < k)
        i++;

    else
        j--;
}
```

---

# Complexity

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n)       |
| Space  | O(n)       |

---

# Important Insight

Transforms:

```txt id="n4m7q2"
BST problem
```

into:

```txt id="g2m8q4"
sorted array two-sum problem
```

---

# Approach 2 — Two BST Iterators (Optimal)

---

# Core Idea

Simulate:

- two pointers directly on BST
- without storing full inorder traversal.

Use:

- inorder iterator → smallest values
- reverse inorder iterator → largest values.

---

# Iterator Meaning

## `next()`

Returns:

- next smallest element.

---

## `before()`

Returns:

- next largest element.

---

# Stack Meaning

## `s1`

Stores:

- inorder traversal state.

---

## `s2`

Stores:

- reverse inorder traversal state.

---

# Push Left Chain

```cpp id="m5q1p7"
void pushLeft(TreeNode* root,
              stack<TreeNode*>& s){

    while(root){

        s.push(root);

        root = root->left;
    }
}
```

---

# Push Right Chain

```cpp id="x4m7q2"
void pushRight(TreeNode* root,
               stack<TreeNode*>& s){

    while(root){

        s.push(root);

        root = root->right;
    }
}
```

---

# next() Iterator

```cpp id="k7q1m5"
int next(stack<TreeNode*>& s){

    TreeNode* node = s.top();

    s.pop();

    pushLeft(node->right, s);

    return node->val;
}
```

---

# before() Iterator

```cpp id="u5m8q2"
int before(stack<TreeNode*>& s){

    TreeNode* node = s.top();

    s.pop();

    pushRight(node->left, s);

    return node->val;
}
```

---

# Main Logic

```cpp id="x2q8m4"
int i = next(s1);

int j = before(s2);

while(i < j){

    if(i + j == k)
        return true;

    else if(i + j < k)
        i = next(s1);

    else
        j = before(s2);
}
```

---

# Complexity

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n)       |
| Space  | O(h)       |

where:

```txt id="w4m7q1"
h = height of BST
```

---

# Why Optimal

Unlike vector approach:

- does NOT store entire inorder traversal.

Only maintains:

- traversal paths in stacks.

---

# Approach Comparison

| Feature      | Vector + Two Pointer | BST Iterators |
| ------------ | -------------------- | ------------- |
| Time         | O(n)                 | O(n)          |
| Space        | O(n)                 | O(h)          |
| Simpler      | ✅                   | harder        |
| More Elegant | okay                 | ✅            |

---

# Important Insight

Iterator approach simulates:

```txt id="m8q2p1"
two pointers directly on BST
```

without converting tree into array.

Very elegant BST pattern.

---

# Common Mistakes

## Iterator Approach

❌ Pushing:

- current node again
  instead of subtree.

Correct:

```cpp id="m8q2p5"
pushLeft(node->right)
```

and

```cpp id="x5m1q7"
pushRight(node->left)
```

---

❌ Using:

- same node twice.

Need:

```cpp id="u4m7q1"
while(i < j)
```

---

# Pattern Recognition

Combination of:

- BST iterator
- inorder traversal
- reverse inorder traversal
- two pointers
- lazy traversal.
