# Predecessor and Successor in BST

Problem : [GeeksforGeeks Problem](https://www.geeksforgeeks.org/problems/predecessor-and-successor/1?utm_source=chatgpt.com)

---

# Definitions

## Predecessor

Largest value:

```txt id="m8q2p5"
< key
```

---

## Successor

Smallest value:

```txt id="x5m1q7"
> key
```

---

# Key BST Property

```txt id="u4m7q1"
left < root < right
```

Allows directional traversal.

---

# Core Idea

While traversing:

- maintain best valid predecessor/successor candidate
- discard invalid subtree immediately using BST property.

---

# Successor Logic

If:

```cpp id="k5m1q8"
node->data > key
```

then:

- current node can be successor
- try finding smaller valid successor in left subtree.

```cpp id="r8p2m5"
succ = node->data;
node = node->left;
```

Otherwise:

```cpp id="n4m7q2"
node = node->right;
```

---

# Predecessor Logic

If:

```cpp id="g2m8q4"
node->data < key
```

then:

- current node can be predecessor
- try finding larger valid predecessor in right subtree.

```cpp id="m5q1p7"
pre = node->data;
node = node->right;
```

Otherwise:

```cpp id="x4m7q2"
node = node->left;
```

---

# Iterative Template

```cpp id="k7q1m5"
TreeNode* node = root;

while(node){

    if(node->data > key){

        succ = node->data;

        node = node->left;
    }
    else{

        node = node->right;
    }
}
```

---

```cpp id="u5m8q2"
node = root;

while(node){

    if(node->data < key){

        pre = node->data;

        node = node->right;
    }
    else{

        node = node->left;
    }
}
```

---

# Complexity

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(h)       |
| Space  | O(1)       |

where:

```txt id="x2q8m4"
h = height of BST
```

---

# Important Insight

BST ordering lets us:

- keep best candidate
- eliminate half search space at every step.

---

# Common Mistakes

❌ Traversing entire tree unnecessarily.

---

❌ Confusing:

- predecessor = immediate smaller
- successor = immediate larger.

---

❌ Using:

- `<=`
- `>=`

incorrectly.

Need:

- strictly smaller
- strictly greater.

---

# Pattern Recognition

Classic BST pattern:

```txt id="w4m7q1"
maintain best valid candidate
while traversing
```

Used in:

- ceil in BST
- floor in BST
- lower bound
- upper bound
- closest value in BST.
