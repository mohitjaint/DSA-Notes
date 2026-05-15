# Delete Node in BST

## BST Property

```txt id="k8m2q5"
left subtree < root < right subtree
```

Deletion must preserve this property.

---

# Recursive Idea

Search for target node using BST property:

```cpp id="u4m7q1"
if(key < root->val)
```

go left.

```cpp id="x5q2m8"
if(key > root->val)
```

go right.

Otherwise:

- node found.

---

# Cases

# Case 1 — Leaf Node

```txt id="m7p1q4"
No children
```

Delete directly:

```cpp id="r2m8q5"
return nullptr;
```

---

# Case 2 — One Child

## Only Right Child

```cpp id="v5x1m7"
return root->right;
```

---

## Only Left Child

```cpp id="u8q2m4"
return root->left;
```

---

# Case 3 — Two Children

## Your Approach (Subtree Merging)

### Idea

1. Take left subtree
2. Find rightmost node in left subtree
3. Attach original right subtree there
4. Return left subtree

---

# Why It Works

Rightmost node in left subtree is:

```txt id="x4m7q2"
largest value smaller than root
```

So attaching right subtree preserves BST ordering.

---

# Two-Children Logic

```cpp id="k7q1m5"
TreeNode* leftSubtree = root->left;

TreeNode* temp = leftSubtree;

while(temp->right){

    temp = temp->right;
}

temp->right = root->right;

return leftSubtree;
```

---

# Complete Recursive Template

```cpp id="u5m8q2"
TreeNode* dfs(TreeNode* root, int key){

    if(!root)
        return nullptr;

    if(key < root->val){

        root->left =
            dfs(root->left, key);
    }
    else if(key > root->val){

        root->right =
            dfs(root->right, key);
    }
    else{

        // leaf node
        if(!root->left &&
           !root->right){

            return nullptr;
        }

        // one child
        else if(!root->left){

            return root->right;
        }
        else if(!root->right){

            return root->left;
        }

        // two children
        else{

            TreeNode* leftSubtree =
                root->left;

            TreeNode* temp =
                leftSubtree;

            while(temp->right){

                temp = temp->right;
            }

            temp->right =
                root->right;

            return leftSubtree;
        }
    }

    return root;
}
```

---

# Complexity

| Metric | Complexity     |
| ------ | -------------- |
| Time   | O(h)           |
| Space  | O(h) recursion |

where:

```txt id="n8q2m5"
h = height of BST
```

---

# Important Insight

BST deletion is difficult because:

- structure changes
- ordering must remain valid.

---

# Alternative Standard Approach

Replace node with:

- inorder successor
  OR
- inorder predecessor

Then recursively delete replacement node.

---

# Common Mistakes

❌ Modifying:

```cpp id="w4m7q1"
root->left
```

while traversing.

Use separate traversal pointer.

---

❌ Losing subtree references.

---

❌ Forgetting returned subtree root.

Recursive deletion changes subtree roots.

---

# Pattern Recognition

BST deletion is a classic:

```txt id="x2q8m4"
Recursive Tree Restructuring
```

problem.
