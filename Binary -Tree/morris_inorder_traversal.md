# Morris Traversal (Inorder Traversal without Stack/Recursion)

## Goal

Perform inorder traversal using:

```txt id="g8m2q5"
O(1) auxiliary space
```

without:

- recursion
- stack.

---

# Inorder Traversal Order

```txt id="w4n7p1"
left → root → right
```

---

# Core Idea

For every node:

- find its inorder predecessor
- temporarily connect predecessor’s right pointer to current node
- later remove the connection

This temporary connection is called:

```txt id="m9q3x6"
thread
```

---

# Inorder Predecessor

For current node:

```txt id="r2k8v4"
curr
```

predecessor is:

```txt id="p7m1q9"
rightmost node in curr->left subtree
```

---

# Cases

# Case 1 — No Left Child

```cpp id="u5x2m8"
if(!curr->left)
```

Visit current node:

```cpp id="n4q7p1"
ans.push_back(curr->val);
```

Move right:

```cpp id="d8m3x5"
curr = curr->right;
```

---

# Case 2 — Left Child Exists

Find predecessor:

```cpp id="t6p1q4"
prev = curr->left;

while(prev->right && prev->right != curr)
    prev = prev->right;
```

---

## Subcase A — Thread Not Created

```cpp id="g3m8x2"
if(prev->right == nullptr)
```

Create thread:

```cpp id="v7q1p5"
prev->right = curr;
```

Move left:

```cpp id="x2m9q6"
curr = curr->left;
```

---

## Subcase B — Thread Already Exists

```cpp id="k4p7m1"
if(prev->right == curr)
```

Remove thread:

```cpp id="n8q2x5"
prev->right = nullptr;
```

Visit current node:

```cpp id="w5m1q8"
ans.push_back(curr->val);
```

Move right:

```cpp id="r9x4p2"
curr = curr->right;
```

---

# Morris Inorder Template

```cpp id="u1m7q5"
while(curr){

    if(!curr->left){

        visit(curr);

        curr = curr->right;
    }
    else{

        find predecessor

        if(thread not created){

            create thread

            curr = curr->left;
        }
        else{

            remove thread

            visit(curr)

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

Even though predecessor search happens repeatedly:

```txt id="q8m3x1"
every edge is traversed at most twice
```

So total work remains O(n).

---

# Important Insight

Morris traversal temporarily modifies tree structure:

```txt id="m5q7p2"
threading the tree
```

but restores it afterward.

Tree remains unchanged finally.

---

# Common Mistakes

❌ Forgetting:

```cpp id="j2x8m4"
curr = curr->right;
```

after visiting node with no left child.

Causes infinite loop.

---

❌ Forgetting to remove thread

```cpp id="v6q1p9"
prev->right = nullptr;
```

Tree gets corrupted.

---

❌ Wrong predecessor search

Must stop when:

```cpp id="x4m7q2"
prev->right == curr
```

---

# Variants

| Traversal | Visit Position         |
| --------- | ---------------------- |
| Inorder   | after removing thread  |
| Preorder  | before creating thread |

---

# Pattern Recognition

Morris Traversal =

```txt id="n7p2q5"
Threaded Binary Tree Traversal
```
