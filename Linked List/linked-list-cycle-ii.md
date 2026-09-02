# Linked List Cycle II — Find the Starting Node of the Cycle

**LeetCode 142**

**Pattern:** `Linked List + Floyd's Cycle Detection (Slow & Fast Pointers)`

---

## Problem

Given the `head` of a linked list, return the node where the cycle begins.

If there is no cycle, return:

```cpp
nullptr
```

Example:

```text
1 → 2 → 3 → 4 → 5
        ↑         │
        └─────────┘
```

The cycle starts at:

```text
3
```

---

# Approach 1: Hash Set

## Idea

Store every visited node in a hash set.

While traversing:

- If the node has not been visited → insert it.
- If we encounter an already visited node → that node is the cycle starting point.

### Code

```cpp
ListNode *detectCycle(ListNode *head) {
    unordered_set<ListNode*> visited;

    ListNode* curr = head;

    while(curr) {
        if(visited.count(curr)) {
            return curr;
        }

        visited.insert(curr);
        curr = curr->next;
    }

    return nullptr;
}
```

### Complexity

```text
Time  : O(n)
Space : O(n)
```

Correct, but not space-optimal.

---

# Approach 2: Floyd's Cycle Detection ⭐ Optimal

This approach has **two phases**.

---

## Phase 1: Detect the Cycle

Initialize:

```cpp
ListNode* slow = head;
ListNode* fast = head;
```

Move:

```text
slow → 1 step
fast → 2 steps
```

```cpp
while(fast && fast->next) {
    slow = slow->next;
    fast = fast->next->next;

    if(slow == fast) {
        // Cycle detected
    }
}
```

If there is no cycle:

```text
fast → nullptr
```

and we return:

```cpp
nullptr
```

---

# Phase 2: Find the Cycle Starting Node

Once:

```cpp
slow == fast
```

we know that a cycle exists.

Now reset one pointer to the head:

```cpp
fast = head;
```

Then move both pointers **one step at a time**:

```cpp
while(slow != fast) {
    slow = slow->next;
    fast = fast->next;
}
```

The node where they meet is the **starting node of the cycle**.

---

# Optimal Code

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {

        ListNode* slow = head;
        ListNode* fast = head;

        // Phase 1: Detect cycle
        while(fast && fast->next) {

            slow = slow->next;
            fast = fast->next->next;

            if(slow == fast) {

                // Phase 2: Find cycle starting node
                fast = head;

                while(slow != fast) {
                    slow = slow->next;
                    fast = fast->next;
                }

                return slow;
            }
        }

        return nullptr;
    }
};
```

---

# Why Does Phase 2 Work?

This is the important mathematical observation.

Let's define:

```text
A = Distance from head to cycle starting node

B = Distance from cycle starting node
    to the slow/fast meeting point

C = Remaining distance in the cycle
    from meeting point back to cycle starting node
```

Visualization:

```text
Head
 ↓
● ─────── A ───────→ Cycle Start
                         ↓
                         ●
                       /   \
                     C       B
                     \       /
                      ←─────
                         ↑
                     Meeting Point
```

Cycle length:

```text
Cycle Length = B + C
```

---

## Distance Travelled

When slow and fast meet:

```text
Slow distance = A + B
```

Fast moves twice as fast:

```text
Fast distance = 2(A + B)
```

The fast pointer may have completed one or more full cycles:

```text
Fast distance = A + B + k(B + C)
```

Since fast moves twice the distance of slow:

```text
2(A + B) = A + B + k(B + C)
```

Therefore:

```text
A + B = k(B + C)
```

Rearranging:

```text
A = k(B + C) - B
```

This simplifies conceptually to:

```text
A ≡ -B (mod Cycle Length)
```

which means:

```text
A ≡ C (mod Cycle Length)
```

### The Key Result

> The distance from `head` to the cycle start is equal to the distance from the meeting point to the cycle start, modulo the cycle length.

Therefore:

```text
Pointer 1 starts at head
        ↓
moves A steps
        ↓
reaches Cycle Start

Pointer 2 starts at meeting point
        ↓
moves A steps
        ↓
also reaches Cycle Start
```

So after resetting:

```cpp
fast = head;
```

and moving both one step at a time:

```cpp
slow = slow->next;
fast = fast->next;
```

they meet exactly at the **cycle starting node**. 🎯

---

# Example

```text
1 → 2 → 3 → 4 → 5
        ↑         │
        └─────────┘
```

Suppose `slow` and `fast` meet somewhere inside the cycle.

Then:

```text
fast → reset to head
slow → remains at meeting point
```

Now:

```text
fast → 1 step
slow → 1 step
```

Eventually:

```text
slow == fast
```

at:

```text
Cycle Starting Node
```

---

# Complexity Comparison

| Approach             |   Time |  Space |
| -------------------- | -----: | -----: |
| Hash Set             | `O(n)` | `O(n)` |
| Floyd's Algorithm ⭐ | `O(n)` | `O(1)` |

---

# Pattern Recognition

When you see:

```text
Linked List + Cycle
```

think:

```text
Slow → 1 step
Fast → 2 steps
```

### If asked:

```text
Does a cycle exist?
```

→ Use Floyd's Cycle Detection.

### If asked:

```text
Where does the cycle start?
```

→ Floyd's Algorithm + Reset one pointer to `head`.

---

# Core Pattern to Memorize

```text
Phase 1:
Slow + Fast
        ↓
Find meeting point

Phase 2:
One pointer → Head
Other pointer → Meeting point
        ↓
Both move 1 step
        ↓
They meet at Cycle Start
```

## Final Complexity

```text
Time  : O(n)
Space : O(1)
```

⭐ **Optimal solution**
