# Add Two Numbers — LeetCode 2

**Pattern:** `Linked List + Simulation + Carry`

---

## Problem

Two non-empty linked lists represent two non-negative integers.

The digits are stored in **reverse order**:

```text
2 → 4 → 3
```

represents:

```text
342
```

Add the two numbers and return the result as a linked list, also in reverse order.

### Example

```text
2 → 4 → 3
5 → 6 → 4
─────────
7 → 0 → 8
```

Because:

```text
342 + 465 = 807
```

---

# Key Observation

The digits are already stored in the order in which normal addition processes them:

```text
ones → tens → hundreds → ...
```

So we can simply traverse both lists from left to right and perform normal addition.

At every position:

```text
digit1 + digit2 + carry
```

Then:

```text
result digit = sum % 10
new carry    = sum / 10
```

---

# Dummy Node Technique

Use a dummy node to simplify construction of the result:

```cpp
ListNode* dummy = new ListNode(0);
ListNode* curr = dummy;
```

Every newly calculated digit is attached after `curr`.

At the end:

```cpp
return dummy->next;
```

---

# Handling Different Lengths

The two lists don't necessarily have the same length.

For example:

```text
9 → 9
1
```

While one list still exists, we continue processing:

```cpp
if(x) sum += x->val;
if(y) sum += y->val;
```

If one pointer becomes `nullptr`, we simply don't add anything from that list.

Conceptually:

```text
x exists → add x->val
y exists → add y->val
neither → add 0
```

---

# Handling Carry

Suppose:

```text
7 + 8 = 15
```

We store:

```text
15 % 10 = 5
```

and carry:

```text
15 / 10 = 1
```

So:

```cpp
ListNode* temp = new ListNode(sum % 10);
sum = sum / 10;
```

---

# Important: Final Carry

Your loop condition is:

```cpp
while(x || y || sum)
```

This is a very good detail.

Why include `sum`?

Consider:

```text
9 → 9
1
```

Eventually:

```text
9 + 1 = 10
```

and after processing the final digit:

```text
x = nullptr
y = nullptr
carry = 1
```

We still need to create:

```text
1
```

Therefore:

```cpp
while(x || y || sum)
```

automatically handles the final carry.

---

# Complete Solution

```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {

        ListNode* x = l1;
        ListNode* y = l2;

        ListNode* dummy = new ListNode(0);
        ListNode* curr = dummy;

        int carry = 0;

        while(x || y || carry) {

            if(x)
                carry += x->val;

            if(y)
                carry += y->val;

            ListNode* temp = new ListNode(carry % 10);

            curr->next = temp;
            curr = temp;

            carry /= 10;

            if(x)
                x = x->next;

            if(y)
                y = y->next;
        }

        return dummy->next;
    }
};
```

Here I renamed your `sum` to `carry` because it represents the **carry between iterations**.

---

# Dry Run

Consider:

```text
l1 = 2 → 4 → 3
l2 = 5 → 6 → 4
```

### First digit

```text
2 + 5 = 7
```

Store:

```text
7
```

Carry:

```text
0
```

---

### Second digit

```text
4 + 6 = 10
```

Store:

```text
0
```

Carry:

```text
1
```

---

### Third digit

```text
3 + 4 + 1 = 8
```

Store:

```text
8
```

Carry:

```text
0
```

Result:

```text
7 → 0 → 8
```

---

# Example With Different Lengths

```text
l1 = 9 → 9
l2 = 1
```

### Step 1

```text
9 + 1 = 10

digit = 0
carry = 1
```

Result:

```text
0
```

### Step 2

```text
9 + 0 + 1 = 10

digit = 0
carry = 1
```

Result:

```text
0 → 0
```

### Step 3

Both lists are finished, but:

```text
carry = 1
```

So the loop executes once more:

```text
digit = 1
```

Final:

```text
0 → 0 → 1
```

which represents:

```text
100
```

---

# Why the Lists Don't Need to Be Reversed

Normally, addition starts from the **least significant digit**.

Here the linked lists already store:

```text
ones → tens → hundreds
```

because the problem gives digits in reverse order.

Therefore:

```text
No reversal needed.
```

This is an important observation.

---

# Common Mistakes

### 1. Forgetting the carry

Wrong:

```cpp
sum = x->val + y->val;
```

Correct:

```cpp
sum = x->val + y->val + carry;
```

---

### 2. Forgetting the final carry

For:

```text
9 + 1
```

the answer is:

```text
0 → 1
```

not just:

```text
0
```

Your:

```cpp
while(x || y || carry)
```

handles this automatically.

---

### 3. Assuming both lists have the same length

Always check:

```cpp
if(x)
if(y)
```

independently.

---

### 4. Returning `dummy`

The dummy node isn't part of the answer.

Return:

```cpp
return dummy->next;
```

---

# Complexity

Let:

```text
n = length of l1
m = length of l2
```

We process each node at most once.

```text
Time: O(max(n, m))
```

The result contains at most:

```text
max(n,m) + 1
```

nodes.

Therefore:

```text
Output Space: O(max(n,m))
Auxiliary Space: O(1)
```

---

# Pattern Recognition

When you see:

```text
Linked List
+
Addition
+
Digits stored in reverse
```

think:

```text
Traverse both lists
       ↓
digit1 + digit2 + carry
       ↓
digit = sum % 10
carry = sum / 10
       ↓
Build result using dummy node
```

---

# Core Insight 🔥

> **Because the digits are stored in reverse order, linked-list traversal naturally processes the numbers from least significant digit to most significant digit. Simply simulate normal addition while carrying overflow to the next node.**

### Memorize this template

```text
sum = digit1 + digit2 + carry

result digit = sum % 10
carry = sum / 10
```

with:

```cpp
while(x || y || carry)
```

**Time: `O(max(n,m))` | Auxiliary Space: `O(1)` | Standard optimal solution.**
