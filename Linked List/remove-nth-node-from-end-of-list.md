# Remove Nth Node From End of List — LeetCode 19

**Pattern:** `Linked List + Two Pointers`

---

## Problem

Given the head of a linked list and an integer `n`, remove the **nth node from the end** of the list and return the head.

### Example

```text
1 → 2 → 3 → 4 → 5,  n = 2

Remove: 4

Result:
1 → 2 → 3 → 5
```

---

# Approach 1: Two Pass Approach

A straightforward approach:

1. Calculate the length of the linked list.
2. Convert `nth from the end` into an index from the beginning.
3. Traverse again and remove the node.

### Complexity

- **Time:** `O(n)`
- **Space:** `O(1)`

This works but requires two traversals.

---

# Approach 2: Two Pointers ⭐ Optimal

## Key Idea

Maintain two pointers:

```text
fast
slow
```

First, move `fast` ahead by `n` nodes.

Then move both pointers together.

This maintains a gap of:

```text
n nodes
```

between them.

---

## Your Approach

### Step 1: Move `fast` `n` steps ahead

```cpp
ListNode* fast = head;

for(int i = 0; i < n; i++){
    fast = fast->next;
}
```

Now there is a gap of `n` nodes between `slow` and `fast`.

---

## Special Case: Remove the Head

If moving `fast` by `n` steps reaches `nullptr`:

```cpp
if(!fast)
```

then:

```text
n == length of linked list
```

Therefore, we need to remove the first node.

```cpp
ListNode* newHead = head->next;
delete head;

return newHead;
```

---

# Step 2: Move Both Pointers

Initialize:

```cpp
ListNode* slow = head;
```

Then:

```cpp
while(fast->next){
    slow = slow->next;
    fast = fast->next;
}
```

When `fast` reaches the last node:

```text
slow
 ↓
1 → 2 → 3 → 4 → 5
              ↑
             fast
```

`slow->next` is the node that must be removed.

---

# Step 3: Remove the Node

Store the node temporarily:

```cpp
ListNode* toDelete = slow->next;
```

Skip it:

```cpp
slow->next = slow->next->next;
```

Delete it:

```cpp
delete toDelete;
```

---

# Complete Solution

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* fast = head;

        // Move fast n steps ahead
        for(int i = 0; i < n; i++) {
            fast = fast->next;
        }

        // Special case: Remove head
        if(!fast) {
            ListNode* newHead = head->next;
            delete head;

            return newHead;
        }

        ListNode* slow = head;

        // Move both pointers together
        while(fast->next) {
            slow = slow->next;
            fast = fast->next;
        }

        // Remove nth node from end
        ListNode* toDelete = slow->next;
        slow->next = slow->next->next;

        delete toDelete;

        return head;
    }
};
```

---

# Dry Run

### Input

```text
1 → 2 → 3 → 4 → 5
n = 2
```

### Move `fast` 2 Steps

```text
slow
 ↓
1 → 2 → 3 → 4 → 5
        ↑
       fast
```

Now move both pointers until `fast` reaches the last node.

```text
          slow
           ↓
1 → 2 → 3 → 4 → 5
                  ↑
                 fast
```

Now:

```text
slow->next = 4
```

So remove node `4`.

Result:

```text
1 → 2 → 3 → 5
```

---

# Why Does This Work?

After moving `fast` ahead by `n` nodes:

```text
Distance(slow → fast) = n
```

We then move both pointers at the same speed.

Therefore, the gap remains constant.

When `fast` reaches the end:

```text
slow is exactly one node before
the nth node from the end.
```

This allows deletion using:

```cpp
slow->next = slow->next->next;
```

---

# Alternative Standard Approach: Dummy Node

A dummy node eliminates the special case for deleting the head.

```cpp
ListNode* dummy = new ListNode(0);
dummy->next = head;

ListNode* slow = dummy;
ListNode* fast = dummy;
```

Move `fast` `n + 1` steps ahead, then move both pointers together.

This approach is often preferred because:

```text
Delete head?
Delete middle node?
Delete last node?

→ All handled using the same logic.
```

But **your approach is completely correct and optimal**.

---

# Complexity

| Complexity  | Value     |
| ----------- | --------- |
| Time        | `O(n)`    |
| Extra Space | `O(1)` ⭐ |

---

# Common Mistakes

### 1. Forgetting the Head Deletion Case

If:

```cpp
fast == nullptr
```

after moving `n` steps:

```text
Remove head.
```

---

### 2. Moving `fast` Only `n - 1` Steps

This changes the required gap and causes an off-by-one error.

---

### 3. Not Saving the Deleted Node

If manually managing memory:

```cpp
delete slow->next;
```

after changing pointers may lose access to the node.

Better:

```cpp
ListNode* toDelete = slow->next;
slow->next = slow->next->next;
delete toDelete;
```

---

# Pattern Recognition

When you see:

```text
Remove kth/nth node from the end
```

Think:

```text
Fast pointer → Move ahead by n nodes
Slow pointer → Start from head

Move both together
        ↓
Fast reaches end
        ↓
Slow reaches node before target
```

---

# Core Insight 🔥

> **Create a fixed gap of `n` nodes between two pointers. When the fast pointer reaches the end, the slow pointer will be positioned immediately before the node that needs to be removed.**

### Final Complexity

```text
Time  : O(n)
Space : O(1) ⭐
```

**Optimal Solution.**
