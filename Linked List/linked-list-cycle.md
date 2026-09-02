# Linked List Cycle — LeetCode 141

**Pattern:** `Linked List + Slow & Fast Pointers (Floyd's Cycle Detection)`

---

## Problem

Given the `head` of a singly linked list, determine whether the linked list contains a cycle.

Example:

```text
1 → 2 → 3 → 4
    ↑       ↓
    └───────┘
```

Return:

```text
true
```

---

# Approach 1: Hash Set

## Idea

Store every visited node inside a hash set.

While traversing:

- If the node has **not been visited**, insert it.
- If the node is encountered **again**, a cycle exists.

### Code

```cpp
bool hasCycle(ListNode* head) {
    unordered_set<ListNode*> visited;

    ListNode* curr = head;

    while(curr) {
        if(visited.count(curr)) {
            return true;
        }

        visited.insert(curr);
        curr = curr->next;
    }

    return false;
}
```

### Complexity

```text
Time  : O(n)
Space : O(n)
```

### Why not optimal?

We need extra memory to store visited nodes.

---

# Approach 2: Floyd's Cycle Detection Algorithm ⭐

## Key Idea

Use two pointers:

```text
slow → moves 1 step at a time
fast → moves 2 steps at a time
```

```text
slow ──→

fast ─────→
```

---

## Standard Implementation

```cpp
class Solution {
public:
    bool hasCycle(ListNode *head) {

        ListNode* slow = head;
        ListNode* fast = head;

        while(fast && fast->next) {

            slow = slow->next;
            fast = fast->next->next;

            if(slow == fast) {
                return true;
            }
        }

        return false;
    }
};
```

---

# Why Does This Work?

## Case 1: No Cycle

Example:

```text
1 → 2 → 3 → 4 → nullptr
```

Since `fast` moves twice as fast:

```text
fast → nullptr
```

Eventually, the loop stops:

```cpp
while(fast && fast->next)
```

Therefore:

```text
No cycle → return false
```

---

## Case 2: Cycle Exists

Example:

```text
        ┌─────────┐
        ↓         │
1 → 2 → 3 → 4 → 5
        ↑         ↓
        └─────────┘
```

Once both pointers enter the cycle:

```text
slow → 1 step
fast → 2 steps
```

The `fast` pointer gains **one position relative to `slow`** during every iteration.

Eventually:

```text
slow
 ↓
● ← fast
```

They meet.

Therefore:

```text
slow == fast
```

means:

```text
Cycle exists ✅
```

---

# Why Will They Eventually Meet?

Imagine the cycle has a length of `k`.

Inside the cycle:

```text
slow moves → 1 step
fast moves → 2 steps
```

So relative to `slow`, `fast` moves:

```text
2 - 1 = 1 step
```

per iteration.

Since the cycle is finite, `fast` will eventually catch up to `slow`.

---

# Why This Loop Condition?

```cpp
while(fast && fast->next)
```

Because we do:

```cpp
fast = fast->next->next;
```

Therefore, we must ensure:

```text
fast != nullptr
```

and:

```text
fast->next != nullptr
```

Otherwise, accessing:

```cpp
fast->next->next
```

would be invalid.

---

# Important Detail: Start Both at `head`

```cpp
ListNode* slow = head;
ListNode* fast = head;
```

Then move them first:

```cpp
slow = slow->next;
fast = fast->next->next;
```

and compare afterward:

```cpp
if(slow == fast)
```

### Why?

If we immediately compare before moving:

```cpp
slow == fast
```

would always be true initially because both pointers start at `head`.

---

# Complexity

## Hash Set Approach

```text
Time  : O(n)
Space : O(n)
```

## Slow & Fast Pointer Approach ⭐

```text
Time  : O(n)
Space : O(1)
```

The slow and fast pointer approach is **optimal**.

---

# Pattern Recognition

Whenever you see:

```text
Linked List + Cycle
```

immediately think:

```text
Slow Pointer → 1 step
Fast Pointer → 2 steps
```

This same Floyd's Cycle Detection pattern is also useful for:

```text
Detect Cycle
Find Cycle Starting Point
Find Cycle Length
```

---

# Core Insight

> **If a fast pointer moves twice as quickly as a slow pointer inside a cycle, it will eventually catch the slow pointer.**

### Pattern to Memorize

```cpp
slow = head;
fast = head;

while(fast && fast->next) {

    slow = slow->next;
    fast = fast->next->next;

    if(slow == fast) {
        return true;
    }
}
```

**Return `true` if they meet; otherwise return `false`.**

### Final Complexity

```text
Time  : O(n)
Space : O(1) ⭐ Optimal
```
