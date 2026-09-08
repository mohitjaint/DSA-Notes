# Odd Even Linked List — LeetCode 328

**Pattern:** `Linked List + Pointer Manipulation`

---

## Problem

Given the head of a singly linked list, group all nodes at:

- **Odd positions first**
- **Even positions afterward**

⚠️ **Odd/even refers to the node's position, not its value.**

### Example

```text
Input:
1 → 2 → 3 → 4 → 5

Positions:
1   2   3   4   5

Output:
1 → 3 → 5 → 2 → 4
```

---

# Key Idea

Maintain two separate chains while traversing the original list:

```text
Odd-position nodes:
1 → 3 → 5

Even-position nodes:
2 → 4
```

At the end:

```text
Odd List → Even List
```

---

# Important Pointers

```cpp
ListNode* odd = head;
ListNode* even = head->next;
ListNode* evenHead = even;
```

### Purpose

| Pointer    | Purpose                                            |
| ---------- | -------------------------------------------------- |
| `odd`      | Points to the last node in the odd-position chain  |
| `even`     | Points to the last node in the even-position chain |
| `evenHead` | Stores the beginning of the even-position chain    |

⚠️ `evenHead` is necessary because `even` keeps moving.

---

# Optimal Approach ⭐

```cpp
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        if(!head || !head->next) return head;

        ListNode* odd = head;
        ListNode* even = head->next;
        ListNode* evenHead = even;

        while(even && even->next) {

            // Connect next odd node
            odd->next = even->next;
            odd = odd->next;

            // Connect next even node
            even->next = odd->next;
            even = even->next;
        }

        // Attach even list after odd list
        odd->next = evenHead;

        return head;
    }
};
```

---

# Dry Run

### Initial List

```text
1 → 2 → 3 → 4 → 5
↑   ↑
odd even
```

Store:

```text
evenHead = 2
```

---

## Iteration 1

### Connect Odd Nodes

```cpp
odd->next = even->next;
```

```text
1 → 3
```

Move `odd`:

```text
odd = 3
```

### Connect Even Nodes

```cpp
even->next = odd->next;
```

```text
2 → 4
```

Move `even`:

```text
even = 4
```

Current structure:

```text
Odd:  1 → 3
Even: 2 → 4
```

---

## Iteration 2

Connect the next odd node:

```text
Odd: 1 → 3 → 5
```

There is no further even node.

Loop stops.

---

# Final Step

```cpp
odd->next = evenHead;
```

Connect:

```text
1 → 3 → 5
          ↓
          2 → 4
```

Final:

```text
1 → 3 → 5 → 2 → 4
```

---

# Why This Loop Condition?

```cpp
while(even && even->next)
```

We need:

```text
even       → current even-position node
even->next → next odd-position node
```

Both must exist before performing:

```cpp
odd->next = even->next;
```

This condition safely handles lists of both odd and even lengths.

---

# Edge Cases

### Empty List

```text
nullptr
```

Return immediately.

---

### One Node

```text
1
```

Already correctly arranged.

---

### Two Nodes

```text
1 → 2
```

Already correctly arranged.

That's why we use:

```cpp
if(!head || !head->next) return head;
```

---

# Complexity

| Complexity | Value     |
| ---------- | --------- |
| Time       | `O(n)`    |
| Space      | `O(1)` ⭐ |

---

# Core Pattern 🔥

```text
odd  → Connect to next odd node
even → Connect to next even node

Finally:
odd → evenHead
```

### Pointer Template to Remember

```cpp
while(even && even->next) {
    odd->next = even->next;
    odd = odd->next;

    even->next = odd->next;
    even = even->next;
}

odd->next = evenHead;
```

> **Separate odd and even positions in-place, preserve their relative order, then connect the odd list to the beginning of the even list.**
