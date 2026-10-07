## Reverse Nodes in k-Group — LeetCode 25

### 🔗 References

- [LeetCode — Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)
- Related: Reverse Linked List
- Related: Reverse Linked List II

---

### 🧩 Pattern

**Find k nodes → isolate → reverse → reconnect**

For:

```text
1 → 2 → 3 → 4 → 5 → 6
k = 3
```

Process group by group:

```text
1 → 2 → 3    4 → 5 → 6
↓ reverse    ↓ reverse
3 → 2 → 1    6 → 5 → 4
```

Result:

```text
3 → 2 → 1 → 6 → 5 → 4
```

If the final group contains fewer than `k` nodes, **leave it unchanged**.

---

### 💡 Key Observation

Before reversing a group, we need to know whether **at least `k` nodes remain**.

That's why:

```cpp
ListNode* kthNode = findKthNode(curr, k - 1);
```

`k - 1` is used because the starting node itself counts as the first node.

For:

```text
curr
 ↓
1 → 2 → 3
```

with `k = 3`:

```text
k - 1 = 2
```

Moving two times reaches node `3`, the kth node.

---

### ⚙️ Algorithm

For every group:

1. Find the kth node.
2. If kth node doesn't exist:
   - Don't reverse the remaining nodes.
   - Connect the previous group's tail to the remaining nodes.
3. Save the node after the group.
4. Disconnect the group.
5. Reverse the group.
6. Connect the reversed group to the previous group.
7. The **old head becomes the new tail**.
8. Move to the next group.

---

### 🧠 Code

```cpp
class Solution {
public:
    ListNode* reverseLL(ListNode* head) {
        ListNode* prev = nullptr;
        ListNode* curr = head;

        while (curr) {
            ListNode* next = curr->next;
            curr->next = prev;
            prev = curr;
            curr = next;
        }

        return prev;
    }

    ListNode* findKthNode(ListNode* head, int k) {
        ListNode* curr = head;

        while (curr && k--) {
            curr = curr->next;
        }

        return curr;
    }

    ListNode* reverseKGroup(ListNode* head, int k) {
        ListNode* curr = head;
        ListNode* prevNode = nullptr;

        while (curr) {

            // Find kth node
            ListNode* kthNode = findKthNode(curr, k - 1);

            // Fewer than k nodes remain
            if (!kthNode) {
                if (prevNode)
                    prevNode->next = curr;
                break;
            }

            // Save next group
            ListNode* nextNode = kthNode->next;

            // Isolate current group
            kthNode->next = nullptr;

            // Reverse current group
            reverseLL(curr);

            // Connect reversed group
            if (curr == head) {
                head = kthNode;
            } else {
                prevNode->next = kthNode;
            }

            // Old head is now the tail
            prevNode = curr;

            // Move to next group
            curr = nextNode;
        }

        return head;
    }
};
```

---

### 🔍 Example

Consider:

```text
1 → 2 → 3 → 4 → 5
k = 2
```

#### Group 1

```text
1 → 2 | 3 → 4 → 5
```

Reverse:

```text
2 → 1 | 3 → 4 → 5
```

#### Group 2

```text
2 → 1 → 3 → 4 | 5
```

Reverse:

```text
2 → 1 → 4 → 3 | 5
```

Only one node remains, so leave it unchanged:

```text
2 → 1 → 4 → 3 → 5
```

---

### 🔑 Critical Pointer Insight

After:

```cpp
reverseLL(curr);
```

the pointers have effectively changed from:

```text
curr
 ↓
1 → 2 → 3
```

to:

```text
3 → 2 → 1
         ↑
        curr
```

So:

```cpp
prevNode = curr;
```

is correct because the **original head `curr` becomes the tail** after reversal.

This is one of the most important things to track in this problem.

---

### ⚠️ Common Mistakes

#### 1. Reversing an incomplete group

Don't reverse unless the kth node exists:

```cpp
if (!kthNode)
    break;
```

---

#### 2. Losing the next group

Always save:

```cpp
ListNode* nextNode = kthNode->next;
```

**before** doing:

```cpp
kthNode->next = nullptr;
```

Otherwise the remaining list becomes inaccessible.

---

#### 3. Forgetting to reconnect groups

After reversing:

```text
3 → 2 → 1
```

the previous group's tail must point to `3`.

```cpp
prevNode->next = kthNode;
```

---

#### 4. Forgetting to update the main head

The first group changes the head:

```text
1 → 2 → 3
```

becomes:

```text
3 → 2 → 1
```

Therefore:

```cpp
head = kthNode;
```

is needed for the first group.

---

### 🎯 Pattern Recognition

When you see:

> **Reverse every k nodes / process a linked list in fixed-size groups**

Think:

**Boundary → Save next → Detach → Reverse → Reconnect**

This pattern is more important than memorizing the exact code.

---

### ⏱️ Complexity

- **Time:** `O(n)`
- **Auxiliary Space:** `O(1)`
- **Optimal:** ✅

Even though `findKthNode()` scans each group and `reverseLL()` scans each group again, every node participates in only a constant number of operations.

---

### 🔥 Core Insight

> **The hardest part isn't reversing the group; it's maintaining the connections before and after the group.**

The three pointers/concepts to always track are:

```text
kthNode   → end of current group
nextNode  → beginning of next group
prevNode  → tail of previous reversed group
```

Once these three are correct, the entire problem becomes straightforward.
