# Construct Binary Tree from Inorder and Postorder Traversal

## Key Observations

### Postorder Traversal

```txt id="cgntku"
Root appears last
```

Structure:

```txt id="6hyk6m"
[left subtree][right subtree][root]
```

---

### Inorder Traversal

```txt id="jlwmn6"
Root splits left and right subtree
```

Structure:

```txt id="jlwmc8"
[left subtree][root][right subtree]
```

---

# Main Idea

1. Last postorder element = root
2. Find root index in inorder
3. Left side of inorder = left subtree
4. Right side of inorder = right subtree
5. Recursively build both subtrees

---

# Recursive Meaning

```cpp id="5jlwmv"
dfs(postL, postR, inL, inR)
```

means:

```txt id="8jlwm1"
Construct subtree using:

postorder[postL : postR)
inorder[inL : inR)
```

(half-open ranges)

---

# Important Formula

Suppose:

```txt id="jlwm9z"
idx = root index in inorder
```

Then:

```txt id="2jlwmw"
leftSize = idx - inL
```

---

# Left Subtree

## Postorder Range

```txt id="5jlwmm"
postL → postL + leftSize
```

---

## Inorder Range

```txt id="7jlwm5"
inL → idx
```

---

# Right Subtree

## Postorder Range

```txt id="0jlwmx"
postL + leftSize → postR - 1
```

(`postR-1` is root)

---

## Inorder Range

```txt id="3jlwmq"
idx+1 → inR
```

---

# Root Selection

```cpp id="8jlwmg"
rootVal = postorder[postR - 1]
```

because root is last element in postorder.

---

# Base Case

```cpp id="9jlwmh"
if(postL >= postR || inL >= inR)
    return nullptr;
```

---

# Hashmap Optimization

Store:

```cpp id="4jlwmr"
unordered_map<int,int> pos;
```

mapping:

```txt id="6jlwmu"
value -> inorder index
```

to avoid repeated searching.

---

# Complexity

## Without Hashmap

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n²)      |
| Space  | O(n)       |

---

## With Hashmap (Optimal)

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n)       |
| Space  | O(n)       |

---

# Common Mistakes

❌ Forgetting root is LAST in postorder

```cpp id="8jlwmk"
postorder[postR - 1]
```

---

❌ Wrong right subtree range

Must exclude root element.

---

❌ Mixing inclusive/exclusive ranges

Be consistent.

---

❌ Linear inorder search every recursion

Use hashmap.

---

# Difference from Preorder + Inorder

| Traversal | Root Position |
| --------- | ------------- |
| Preorder  | first         |
| Postorder | last          |

Everything else remains similar.

---

# Pattern Recognition

This is another:

```txt id="0jlwm4"
Tree Construction + Recursive Range Splitting
```

problem.
