# Serialize and Deserialize Binary Tree

## Key Idea

Convert tree into a reversible string representation.

Requirement:

```txt id="f8m2q7"
deserialize(serialize(root))
```

should reconstruct the same tree.

---

# Standard Approach

## Preorder Traversal + Null Markers

Traversal order:

```txt id="z4n7p1"
root → left → right
```

Use:

```txt id="k2x8m5"
#
```

to represent null nodes.

---

# Why Null Markers Are Needed

Without null markers:

```txt id="v7q1r4"
1,2
```

is ambiguous.

Could represent:

```txt id="h9m4p2"
  1
 /
2
```

OR

```txt id="r3x7k8"
1
 \
  2
```

Null markers preserve exact structure.

---

# Example

Tree:

```txt id="m1q8v6"
      1
     / \
    2   3
```

Serialized:

```txt id="d7k2p5"
1,2,#,#,3,#,#
```

---

# Serialization Logic

## DFS

```cpp id="u4m9x1"
if(!root){

    s += "#,";
    return;
}
```

Otherwise:

```cpp id="q7p3n8"
add node value
serialize left
serialize right
```

---

# Serialization Template

```cpp id="t2k8m4"
void dfs(TreeNode* root, string& s){

    if(!root){

        s += "#,";
        return;
    }

    s += to_string(root->val) + ",";

    dfs(root->left, s);
    dfs(root->right, s);
}
```

---

# Deserialization Logic

Read tokens sequentially.

If token:

```txt id="x5m1q9"
#
```

return nullptr.

Otherwise:

1. create node
2. recursively build left subtree
3. recursively build right subtree

---

# Deserialization Template

```cpp id="r8n4v2"
TreeNode* build(stringstream& ss){

    string token;

    getline(ss, token, ',');

    if(token == "#")
        return nullptr;

    TreeNode* root =
        new TreeNode(stoi(token));

    root->left = build(ss);
    root->right = build(ss);

    return root;
}
```

---

# Main Functions

```cpp id="w3m7q1"
string serialize(TreeNode* root)
```

returns serialized string.

---

```cpp id="p6k2x9"
TreeNode* deserialize(string data)
```

reconstructs tree.

---

# Complexity

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n)       |
| Space  | O(n)       |

Optimal.

---

# Important Insight

Serialization problems are about:

```txt id="n4q8m3"
Designing a reversible representation
```

NOT necessarily binary encoding.

---

# Common Mistakes

❌ Forgetting null markers

Tree structure becomes ambiguous.

---

❌ Wrong traversal order during deserialization

Must match serialization order exactly.

---

❌ Not consuming tokens sequentially

Parsing becomes incorrect.

---

# Pattern Recognition

This is a classic:

```txt id="g7m1p5"
DFS + stream parsing
```

problem.
