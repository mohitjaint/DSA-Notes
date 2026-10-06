## Add One to a Number Represented by Linked List

### 🔗 Reference

- **LeetCode:** Add Two Numbers is a related pattern; this problem is commonly found on GFG as **Add 1 to a Linked List**.
- Related: Reverse Linked List

---

### 🧩 Pattern

**Reverse → Process Carry → Reverse back**

When a number is represented as:

```text
1 → 2 → 9
```

the **most significant digit is at the head**, but addition starts from the **least significant digit**.

So reverse the list first.

---

### 💡 Key Observation

We need to add `1` starting from the **last node**.

Instead of traversing to the end repeatedly, reverse the list:

```text
Original:
1 → 2 → 9

Reverse:
9 → 2 → 1
↑
start addition
```

Then perform normal decimal addition using `carry`.

---

### ⚙️ Algorithm

1. Reverse the linked list.
2. Set `carry = 1`.
3. Traverse while `curr != nullptr` and `carry != 0`.
4. Calculate:
   ```text
   sum = curr->data + carry
   ```
5. Update:
   ```text
   curr->data = sum % 10
   carry = sum / 10
   ```
6. Reverse the list again.
7. If `carry` is still `1`, create a new head.

---

### 🧠 Your Optimal Code

```cpp
class Solution {
public:
    Node* reverseLL(Node* head) {
        Node* curr = head;
        Node* prev = nullptr;

        while (curr) {
            Node* next = curr->next;
            curr->next = prev;

            prev = curr;
            curr = next;
        }

        return prev;
    }

    Node* addOne(Node* head) {

        int carry = 1;

        head = reverseLL(head);
        Node* curr = head;

        while (carry && curr) {

            int sum = curr->data + carry;

            curr->data = sum % 10;
            carry = sum / 10;

            curr = curr->next;
        }

        head = reverseLL(head);

        if (carry) {
            Node* newHead = new Node(carry);
            newHead->next = head;
            head = newHead;
        }

        return head;
    }
};
```

---

### 🔍 Example 1

```text
Input:
1 → 2 → 9
```

Reverse:

```text
9 → 2 → 1
```

Add `1`:

```text
9 + 1 = 10
```

So:

```text
0 → 3 → 1
```

Reverse:

```text
1 → 3 → 0
```

**Answer:**

```text
1 → 3 → 0
```

---

### 🔍 Example 2 — Important Edge Case

```text
9 → 9 → 9
```

Reverse:

```text
9 → 9 → 9
```

Process:

```text
9 + 1 = 10  → digit 0, carry 1
9 + 1 = 10  → digit 0, carry 1
9 + 1 = 10  → digit 0, carry 1
```

After reversing:

```text
0 → 0 → 0
```

Carry is still `1`, so create a new head:

```text
1 → 0 → 0 → 0
```

---

### ⏱️ Complexity

**Time:** `O(n)`

There are at most three linear traversals:

- Reverse
- Add
- Reverse

So:

```text
O(n) + O(n) + O(n) = O(n)
```

**Auxiliary Space:** `O(1)`

Only pointers and `carry` are used.

One extra node may be created when there is a final carry, but that is part of the output rather than auxiliary storage.

---

### ⚠️ Common Mistakes

1. **Forgetting to reverse back**

   ```cpp
   head = reverseLL(head);
   ```

2. **Forgetting the final carry**

   `999 + 1` requires:

   ```text
   1 → 0 → 0 → 0
   ```

3. **Continuing unnecessarily after `carry = 0`**

   Once carry becomes zero, the remaining digits are already correct:

   ```cpp
   while (carry && curr)
   ```

4. **Incorrect reversal implementation**

   Always save `next` before changing `curr->next`.

---

### 🎯 Pattern Recognition

Whenever you see:

> **Number represented by a linked list + arithmetic starting from the last digit**

Think:

**Reverse → Perform arithmetic → Reverse back**

This same idea can be extended to:

- Add two numbers represented by linked lists
- Subtract numbers represented by linked lists
- Multiply numbers represented by linked lists

---

### 🔑 Core Insight

> **Linked lists give easy access to the front, but arithmetic starts from the back. Reverse the list to make the least significant digit accessible first.**
