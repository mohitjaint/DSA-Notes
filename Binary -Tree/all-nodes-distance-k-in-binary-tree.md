# All Nodes Distance K in Binary Tree

## Key Observation

Need movement in:

- left
- right
- parent

So convert tree into an undirected graph by storing parent links.

---

# Main Idea

1. Store parent of every node
2. Start BFS from target node
3. BFS level = distance from target
4. Nodes at level `k` = answer

---

# Parent Mapping

```cpp id="87j8fg"
parent[child] = node;
```

---

# Parent DFS

```cpp id="p2h8d3"
void findParents(unordered_map<TreeNode*, TreeNode*>& parent,
                 TreeNode* root){

    if(!root) return;

    if(root->left){

        parent[root->left] = root;
        findParents(parent, root->left);
    }

    if(root->right){

        parent[root->right] = root;
        findParents(parent, root->right);
    }
}
```

---

# BFS Idea

Start from:

```cpp id="ofgn06"
q.push(target);
```

At every step:

- go to left child
- right child
- parent

Use visited set to avoid revisiting.

---

# BFS Template

```cpp id="igj1o8"
while(!q.empty()){

    if(dist == k)
        break;

    int size = q.size();

    for(int i = 0; i < size; i++){

        TreeNode* node = q.front();
        q.pop();

        process neighbours
    }

    dist++;
}
```

---

# Important BFS Observation

```txt id="9en4eq"
One BFS level = one unit distance
```

---

# Visited Set

Necessary because:

```txt id="yj0d33"
parent <-> child
```

creates cycles.

Without visited set:

- infinite revisits happen.

---

# Neighbours

```cpp id="byvgbm"
parent[node]
node->left
node->right
```

---

# Complexity

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n)       |
| Space  | O(n)       |

---

# Common Mistakes

❌ Forgetting:

```cpp id="e0hkxj"
dist++
```

after one BFS level.

---

❌ Missing visited set

Causes infinite traversal.

---

❌ Comparing node values instead of pointers

Use:

```cpp id="wdg9yt"
TreeNode*
```

comparisons.

---

❌ Forgetting parent traversal

Need all 3 directions:

- left
- right
- parent

---

# Pattern Recognition

This is:

```txt id="v8y7im"
Graph BFS on a tree
```

Tree problems often become easier after:

- adding parent links
- treating tree as graph.
