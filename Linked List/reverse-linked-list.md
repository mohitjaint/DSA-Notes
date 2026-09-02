# Reverse Linked List — LeetCode 206

## Reference

**Question:** [https://leetcode.com/problems/reverse-linked-list/](https://leetcode.com/problems/reverse-linked-list/)

**Pattern:**
`Linked List + Iterative Pointer Manipulation`

---

# Problem

Given the head of a singly linked list, reverse the list and return the new head.

Example:

```text
1 → 2 → 3 → 4 → 5 → nullptr
```

becomes:

```text
5 → 4 → 3 → 2 → 1 → nullptr
```

---

# Key Observation

To reverse a linked list, we need to change every node's:

```cpp
curr->next
```

from pointing **forward** to pointing **backward**.

The danger is:

> If we change `curr->next` before saving its original value, we lose access to the remaining list.

Therefore, we need **three pointers**:

```text
prev
curr
next
```

---

# The Standard Pattern

For every node:

```text
1. Save next
2. Reverse current pointer
3. Move prev
4. Move curr
```

In code:

```cpp
next = curr->next;
curr->next = prev;
prev = curr;
curr = next;
```

This sequence is the most important thing to remember.

---

# Initial State

```cpp
ListNode* curr = head;
ListNode* prev = nullptr;
```

For:

```text
1 → 2 → 3 → nullptr
```

we have:

```text
prev = nullptr
curr = 1
```

---

# Iteration 1

Save the original next:

```cpp
next = curr->next;
```

```text
next = 2
```

Reverse:

```cpp
curr->next = prev;
```

Now:

```text
1 → nullptr
```

Move pointers:

```cpp
prev = curr;
curr = next;
```

State:

```text
prev        curr
 ↓           ↓
 1 → null    2 → 3 → null
```

---

# Iteration 2

Save:

```text
next = 3
```

Reverse:

```text
2 → 1 → null
```

Move:

```text
prev = 2
curr = 3
```

---

# Iteration 3

Save:

```text
next = nullptr
```

Reverse:

```text
3 → 2 → 1 → null
```

Then:

```text
curr = nullptr
```

The loop ends.

`prev` is now the new head:

```text
prev
 ↓
3 → 2 → 1 → nullptr
```

---

# Code

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {

        ListNode* curr = head;
        ListNode* prev = nullptr;
        ListNode* next;

        while(curr != nullptr){

            // Save remaining list
            next = curr->next;

            // Reverse current pointer
            curr->next = prev;

            // Move prev forward
            prev = curr;

            // Move curr forward
            curr = next;
        }

        return prev;
    }
};
```

---

# Why `prev = nullptr`?

The original head becomes the **last node** after reversal.

Therefore, its new `next` must be:

```text
nullptr
```

Initializing:

```cpp
ListNode* prev = nullptr;
```

makes this happen naturally on the first iteration:

```cpp
curr->next = prev;
```

No special handling for the first node is required.

---

# Why Do We Need `next`?

Consider:

```text
1 → 2 → 3
```

If we directly do:

```cpp
curr->next = prev;
```

while `prev = nullptr`, we get:

```text
1 → nullptr
```

Without first saving:

```cpp
next = curr->next;
```

we lose access to:

```text
2 → 3
```

Therefore:

> **Always save `curr->next` before modifying it.**

---

# Why Does `prev` Become the Answer?

When the loop finishes:

```text
curr = nullptr
```

because we've processed every node.

The last processed node is stored in:

```cpp
prev
```

Therefore:

```cpp
return prev;
```

returns the new head.

---

# Common Mistake

### Moving `curr` without saving `next`

Wrong:

```cpp
curr->next = prev;
curr = curr->next;
```

After reversing the pointer, `curr->next` points backward, so you've lost the original forward direction.

Correct:

```cpp
next = curr->next;
curr->next = prev;
prev = curr;
curr = next;
```

---

# Common Mistake From Stack Approach

A stack-based solution can reverse the list:

```text
Time  : O(n)
Space : O(n)
```

but the optimal solution doesn't need extra storage.

The three-pointer approach uses:

```text
Time  : O(n)
Space : O(1)
```

---

# Complexity

Every node is processed exactly once:

```text
Time: O(n)
```

Only three pointers are used:

```text
prev
curr
next
```

Therefore:

```text
Space: O(1)
```

This is **optimal**.

You cannot improve the time below `O(n)` because every node's pointer has to be changed.

---

# Recursive Alternative

There is also a recursive solution, but for this problem the iterative solution is generally preferable because it uses:

```text
O(1) auxiliary space
```

instead of recursion stack space.

---

# Pattern Recognition

Whenever you need to **reverse or manipulate links in a singly linked list**, think about pointer relationships rather than values.

For reversal:

```text
prev ← curr → next
```

becomes:

```text
prev ← curr ← next
```

The standard pointer movement is:

```text
Save next
    ↓
Reverse curr
    ↓
Move prev
    ↓
Move curr
```

---

# Core Insight

> **Never change a linked-list pointer before saving the pointer you still need.**

For reversal:

```text
next = curr->next
        ↓
curr->next = prev
        ↓
prev = curr
        ↓
curr = next
```

Memorize this **four-step sequence**, not just the code.

### Final Pattern

```text
prev = nullptr
curr = head

while(curr):
    next = curr->next
    curr->next = prev
    prev = curr
    curr = next

return prev
```

**O(n) time + O(1) space = optimal.**
