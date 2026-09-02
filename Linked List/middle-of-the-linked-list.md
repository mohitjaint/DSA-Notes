# Middle of the Linked List — LeetCode 876

**Pattern:** `Linked List + Two Pointers (Slow & Fast)`

---

## Problem

Given the head of a singly linked list, return the **middle node**.

If there are two middle nodes (even number of nodes), return the **second middle node**.

Example:

```text
1 → 2 → 3 → 4 → 5
        ↑
      middle
```

Answer:

```text
3
```

For an even-sized list:

```text
1 → 2 → 3 → 4 → 5 → 6
        ↑   ↑
      first second
            middle
```

Answer:

```text
4
```

---

# Key Idea — Slow & Fast Pointers

Use two pointers:

```cpp
ListNode *slow = head;
ListNode *fast = head;
```

- `slow` moves **1 step**
- `fast` moves **2 steps**

```cpp
slow = slow->next;
fast = fast->next->next;
```

Because `fast` moves twice as quickly, when `fast` reaches the end, `slow` will be at the middle.

---

# Code

```cpp
class Solution {
public:
    ListNode* middleNode(ListNode* head) {

        ListNode *slow = head;
        ListNode *fast = head;

        while(fast != nullptr && fast->next != nullptr){

            slow = slow->next;
            fast = fast->next->next;
        }

        return slow;
    }
};
```

---

# Why Does It Work?

Consider:

```text
1 → 2 → 3 → 4 → 5
```

Initially:

```text
slow = 1
fast = 1
```

### Iteration 1

```text
slow → 2
fast → 3
```

### Iteration 2

```text
slow → 3
fast → 5
```

Now:

```text
fast->next == nullptr
```

Stop.

```text
slow = 3
```

Therefore, `3` is the middle.

---

# Even Number of Nodes

Consider:

```text
1 → 2 → 3 → 4 → 5 → 6
```

### Initially

```text
slow = 1
fast = 1
```

### After 1 iteration

```text
slow = 2
fast = 3
```

### After 2

```text
slow = 3
fast = 5
```

### After 3

```text
slow = 4
fast = nullptr
```

Therefore:

```text
slow = 4
```

which is the **second middle node**.

This happens naturally because both pointers start at `head`.

---

# Why the Loop Condition?

```cpp
while(fast != nullptr && fast->next != nullptr)
```

We need both conditions because we're doing:

```cpp
fast = fast->next->next;
```

If `fast->next` is `nullptr`, accessing:

```cpp
fast->next->next
```

would be invalid.

So:

```text
fast != nullptr
AND
fast->next != nullptr
```

ensures the two-step movement is safe.

---

# Complexity

For `n` nodes:

```text
Time  : O(n)
Space : O(1)
```

Only a single traversal is required, and we use two pointers.

This is the **standard and optimal solution**.

---

# Common Alternative — Count Length

You could first calculate the length:

```text
1st traversal → find n
2nd traversal → move to n/2
```

This takes:

```text
Time: O(n)
Space: O(1)
```

but requires **two traversals**.

Slow/fast pointers accomplish it in a single traversal:

```text
fast → 2 steps
slow → 1 step
```

So this is the preferred approach.

---

# Pattern Recognition

Whenever you need to find something around the **middle of a linked list**, immediately think:

```text
Slow & Fast Pointer
```

Common applications:

```text
Middle of Linked List
        ↓
Detect Cycle
        ↓
Find Cycle Starting Point
        ↓
Palindrome Linked List
        ↓
Reorder Linked List
```

The fundamental pattern:

```text
slow → 1 step
fast → 2 steps
        ↓
fast reaches end
        ↓
slow reaches middle
```

---

# Core Insight

> **If one pointer moves twice as fast as another, then when the fast pointer has traversed the list, the slow pointer has traversed roughly half of it.**

For this problem:

```text
fast = 2 × slow
        ↓
fast reaches end
        ↓
slow is at middle
```

**Standard solution: O(n) time, O(1) space.**
