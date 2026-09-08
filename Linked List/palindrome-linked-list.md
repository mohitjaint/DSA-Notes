# Palindrome Linked List — LeetCode 234

**Pattern:** `Linked List + Slow/Fast Pointers + Reversal`

---

## Problem

Given the head of a singly linked list, determine whether it is a palindrome.

A palindrome reads the same forward and backward.

### Example

```text
1 → 2 → 2 → 1

Palindrome → true
```

```text
1 → 2 → 3

Not Palindrome → false
```

---

# Optimal Approach ⭐

The idea consists of **three steps**:

```text
1. Find the middle of the linked list
2. Reverse the second half
3. Compare both halves
```

---

# Step 1: Find the End of the First Half

Use:

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
while(fast->next && fast->next->next) {
    slow = slow->next;
    fast = fast->next->next;
}
```

This places `slow` at the **end of the first half**.

---

## Even-Length Example

```text
1 → 2 → 2 → 1
    ↑
   slow
```

So:

```text
First Half:   1 → 2
Second Half:  2 → 1
```

The second half starts at:

```cpp
slow->next
```

---

## Odd-Length Example

```text
1 → 2 → 3 → 2 → 1
    ↑
   slow
```

Here, `slow` ends at the middle node.

We reverse:

```cpp
slow->next
```

which gives:

```text
First comparison:  1 → 2
Second half:       1 → 2
```

The middle element does not need to be compared.

---

# Step 2: Reverse the Second Half

Use the standard linked list reversal:

```cpp
ListNode* reverseLL(ListNode* head) {
    ListNode* curr = head;
    ListNode* prev = nullptr;

    while(curr) {
        ListNode* next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
    }

    return prev;
}
```

Reverse:

```cpp
ListNode* secondHalf = reverseLL(slow->next);
```

---

## Example

Original:

```text
1 → 2 → 2 → 1
```

Second half:

```text
2 → 1
```

After reversal:

```text
1 → 2
```

Now we can compare:

```text
First Half:          1 → 2
Reversed Second Half: 1 → 2
```

---

# Step 3: Compare Both Halves

```cpp
ListNode* first = head;
ListNode* second = secondHalf;

while(second) {
    if(first->val != second->val) {
        return false;
    }

    first = first->next;
    second = second->next;
}
```

We only traverse until `second` becomes `nullptr`.

This is important because:

- For an odd-length list, the first half contains the middle node.
- We don't need to compare the middle node with anything.

---

# Complete Solution

```cpp
class Solution {
public:

    ListNode* reverseLL(ListNode* head) {
        ListNode* curr = head;
        ListNode* prev = nullptr;

        while(curr) {
            ListNode* next = curr->next;
            curr->next = prev;
            prev = curr;
            curr = next;
        }

        return prev;
    }

    bool isPalindrome(ListNode* head) {

        if(!head || !head->next) return true;

        // Step 1: Find end of first half
        ListNode* slow = head;
        ListNode* fast = head;

        while(fast->next && fast->next->next) {
            slow = slow->next;
            fast = fast->next->next;
        }

        // Step 2: Reverse second half
        ListNode* secondHalf = reverseLL(slow->next);

        // Step 3: Compare both halves
        ListNode* first = head;
        ListNode* second = secondHalf;

        while(second) {
            if(first->val != second->val) {
                return false;
            }

            first = first->next;
            second = second->next;
        }

        return true;
    }
};
```

---

# Dry Run

### Input

```text
1 → 2 → 3 → 2 → 1
```

### Step 1: Find Middle

```text
1 → 2 → 3 → 2 → 1
        ↑
       slow
```

### Step 2: Reverse After `slow`

```text
Original second part:

2 → 1
```

After reversal:

```text
1 → 2
```

The list structure for comparison becomes:

```text
First pointer:   1 → 2 → 3
Second pointer:  1 → 2
```

### Step 3: Compare

```text
1 == 1 ✅
2 == 2 ✅
```

All nodes in the second half match.

```text
Palindrome → true
```

---

# Why Is `while(second)` Used?

```cpp
while(second)
```

The second half is always the same size or smaller than the first half.

Therefore, we only need to ensure:

```text
Every node in the reversed second half
matches the corresponding node from the beginning.
```

---

# Complexity

| Complexity  | Value     |
| ----------- | --------- |
| Time        | `O(n)`    |
| Extra Space | `O(1)` ⭐ |

---

# Pattern Recognition

When you see:

```text
Linked List + Palindrome
```

Think:

```text
Slow/Fast Pointer
        ↓
Find Middle
        ↓
Reverse Second Half
        ↓
Compare Both Halves
```

---

# Core Insight 🔥

> **A linked list can be checked for palindrome without extra space by reversing its second half and comparing it with the first half.**

### Pattern to Remember

```text
Find Middle
    ↓
Reverse Second Half
    ↓
Compare First & Second Half
```

**`O(n)` Time + `O(1)` Extra Space → Optimal Solution ⭐**
