# Morris Preorder Traversal

## Goal

Perform preorder traversal using:

```txt id="u8m2q5"
O(1) auxiliary space
```

without:

- recursion
- stack.

---

# Preorder Traversal Order

```txt id="m4q7p1"
root → left → right
```

---

# Core Idea

Use temporary threads in tree.

For every node:

- find preorder predecessor
- temporarily connect predecessor’s right to current node
- later remove connection

---

# Predecessor

For current node:

```txt id="x7m1q4"
curr
```

predecessor is:

```txt id="k2p8m5"
rightmost node in curr->left subtree
```

---

# Cases

# Case 1 — No Left Child

Visit node:

```cpp id="r5m9q2"
ans.push_back(curr->val);
```

Move right:

```cpp id="u3x7m1"
curr = curr->right;
```

---

# Case 2 — Left Child Exists

Find predecessor:

```cpp id="q8m2p4"
TreeNode* prev = curr->left;

while(prev->right &&
      prev->right != curr){

    prev = prev->right;
}
```

---

## Subcase A — Thread Not Created

```cpp id="n7q1m5"
if(prev->right == nullptr)
```

### Steps

1. Visit current node
2. Create thread
3. Move left

```cpp id="v4m8q2"
ans.push_back(curr->val);

prev->right = curr;

curr = curr->left;
```

---

## Subcase B — Thread Already Exists

```cpp id="d2x7m9"
if(prev->right == curr)
```

### Steps

1. Remove thread
2. Move right

```cpp id="g5m1q8"
prev->right = nullptr;

curr = curr->right;
```

---

# Morris Preorder Template

```cpp id="t8q2m4"
while(curr){

    if(!curr->left){

        visit(curr);

        curr = curr->right;
    }
    else{

        find predecessor

        if(thread not created){

            visit(curr)

            create thread

            curr = curr->left;
        }
        else{

            remove thread

            curr = curr->right;
        }
    }
}
```

---

# Complexity

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n)       |
| Space  | O(1)       |

---

# Why Time is O(n)

Every edge is traversed at most twice:

- once while creating thread
- once while removing thread.

---

# Difference from Morris Inorder

| Traversal | Visit Timing           |
| --------- | ---------------------- |
| Inorder   | after removing thread  |
| Preorder  | before creating thread |

This is the key difference.

---

# Important Insight

Morris traversal temporarily modifies tree structure but restores it afterward.

Final tree remains unchanged.

---

# Common Mistakes

❌ Starting predecessor search from:

```cpp id="y7m2q5"
curr
```

Must start from:

```cpp id="w4p8m1"
curr->left
```

---

❌ Forgetting to remove thread

```cpp id="n5x1q7"
prev->right = nullptr;
```

---

❌ Visiting node at wrong time

For preorder:

- visit BEFORE going left.

---

# Pattern Recognition

Morris Traversal =

```txt id="k8m3q2"
Threaded Binary Tree Traversal
```
