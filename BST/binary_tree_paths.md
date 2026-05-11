line : [leetcode]("https://leetcode.com/problems/binary-tree-paths/description/")

---

# Binary Tree Paths — Notes

## Core Idea

Use DFS + backtracking to build root-to-leaf paths.

---

## Pattern Used

### DFS Backtracking Pattern

```cpp
save_state()

make_changes()

recurse()

restore_state()
```

Applied here:

```cpp
int len = path.size();

path += to_string(root->val);

dfs(...)

path.resize(len);
```

---

## Important Optimization

### Bad

```cpp
s = s.substr(0, l);
```

Why bad?

- Creates a new string
- Copies characters
- Causes repeated allocations

Complexity: O(length copied)

---

### Better

```cpp
s.resize(l);
```

Why better?

- Reuses same string buffer
- No new allocation while shrinking
- Efficient backtracking

Usually O(1) when shrinking.

---

## Key Insight

Instead of creating new strings during recursion,
modify one string in-place and restore its old state after DFS.

---

## Leaf Node Logic

```cpp
if (!root->left && !root->right)
```

This means current node is a leaf.

Push current path directly into answer.

---

## Correct Arrow Handling

Append `"->"` only for non-leaf nodes.

```cpp
if (!leaf)
    path += "->";
```

Avoids trimming later.

---

## Final Optimized Code

```cpp
class Solution {
public:
    void dfs(TreeNode* root, string& path, vector<string>& ans) {
        if (!root) return;

        int len = path.size();

        path += to_string(root->val);

        if (!root->left && !root->right) {
            ans.push_back(path);
        } else {
            path += "->";

            dfs(root->left, path, ans);
            dfs(root->right, path, ans);
        }

        path.resize(len);
    }

    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> ans;
        string path;

        dfs(root, path, ans);

        return ans;
    }
};
```

---

## Complexity

### Time

```text
O(total characters in output)
```

Optimal because all paths must be generated.

---

### Space

```text
O(height of tree)
```

due to recursion stack.

---

## Mistakes / Learnings

- `substr()` is expensive inside recursion
- Always restore state after recursive calls
- `nullptr` is preferred over `NULL` in modern C++
- Backtracking with `resize()` is a common optimization pattern
