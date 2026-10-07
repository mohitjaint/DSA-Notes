## Rotate List — LeetCode 61

### 🔗 References

- [LeetCode — Rotate List](https://leetcode.com/problems/rotate-list/)
- Related: Reverse Linked List
- Related: Reverse Nodes in k-Group

---

### 🧩 Pattern

**Find length → Reduce `k` → Find new tail → Cut → Connect**

For:

```text
1 → 2 → 3 → 4 → 5
k = 2
```

Rotate right by 2:

```text
4 → 5 → 1 → 2 → 3
```

The important observation is that the **last `k` nodes move to the front**.

---

### 💡 Key Observation

Rotating by the list's length changes nothing.

For:

```text
1 → 2 → 3 → 4 → 5
```

Rotating by:

```text
5, 10, 15, ...
```

always gives the same list.

Therefore:

```cpp
k %= size;
```

This is essential when `k` is very large.

---

### ⚙️ Algorithm

1. Traverse the list to find:
   - `size`
   - `last` node
2. If the list is empty, return it.
3. Reduce:
   ```cpp
   k %= size;
   ```
4. If `k == 0`, return the original head.
5. Find the **new tail**, which is at position:
   ```text
   size - k - 1
   ```
   using 0-based indexing.
6. The node after the new tail becomes the new head.
7. Break the list after the new tail.
8. Connect the old tail to the old head.

---

### 🧠 Code

```cpp
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        int size = 0;

        ListNode* curr = head;
        ListNode* last;

        while (curr) {
            size++;

            if (curr->next == nullptr)
                last = curr;

            curr = curr->next;
        }

        if (size == 0)
            return head;

        k %= size;

        if (k == 0)
            return head;

        curr = head;

        int i = 0;

        while (i != size - k - 1) {
            curr = curr->next;
            i++;
        }

        ListNode* newHead = curr->next;

        curr->next = nullptr;
        last->next = head;

        return newHead;
    }
};
```

---

### 🔍 Example

```text
1 → 2 → 3 → 4 → 5
k = 2
```

Length:

```text
n = 5
```

New tail position:

```text
n - k - 1
= 5 - 2 - 1
= 2
```

So the new tail is:

```text
1 → 2 → 3 → 4 → 5
        ↑
     new tail
```

The node after it is the new head:

```text
4 → 5
↑
new head
```

Cut:

```text
1 → 2 → 3    4 → 5
```

Connect old tail to old head:

```text
4 → 5 → 1 → 2 → 3
```

---

### ⚠️ Common Mistakes

#### 1. Forgetting `k %= size`

If:

```text
n = 5
k = 102
```

you only need:

```text
102 % 5 = 2
```

---

#### 2. Finding the wrong node

The **new tail** is:

```text
size - k - 1
```

not:

```text
size - k
```

because the node at `size - k` becomes the **new head**.

---

#### 3. Forgetting to break the list

You need:

```cpp
curr->next = nullptr;
```

Otherwise the old connection remains.

---

#### 4. Forgetting to connect the old tail

After cutting:

```text
1 → 2 → 3    4 → 5
```

connect:

```cpp
last->next = head;
```

to obtain:

```text
4 → 5 → 1 → 2 → 3
```

---

### 🎯 Pattern Recognition

When you see:

> **Rotate a linked list right by `k`**

Think:

**Last `k` nodes → front**

Then:

```text
Find length
     ↓
k %= n
     ↓
Find new tail
     ↓
Cut
     ↓
Old tail → old head
```

---

### 🔑 Core Insight

> **Don't actually perform `k` rotations. Find the point where the list must be cut once.**

For a list of length `n`, rotating right by `k` means:

```text
new tail = node at index (n - k - 1)
new head = node at index (n - k)
```

That converts what could be `O(nk)` repeated rotations into a single **O(n)** operation.

---

### ⏱️ Complexity

- **Time:** `O(n)`
- **Auxiliary Space:** `O(1)`
- **Optimal:** ✅
