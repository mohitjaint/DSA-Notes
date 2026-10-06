## Intersection of Two Linked Lists — LeetCode 160

### 🔗 References

- [LeetCode — Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)
- Related: Linked List Cycle II, Middle of Linked List

---

### 🧩 Pattern

**Two Pointers + Switch Heads**

When two linked lists may have different lengths, make each pointer traverse **both lists**.

```text
curr1: A → B
curr2: B → A
```

This eliminates the difference in their lengths.

---

### 💡 Key Observation

Suppose:

```text
A: a1 → a2 → a3 ─┐
                 ├→ c1 → c2
B:      b1 → b2 ─┘
```

The pointers initially have different distances to the intersection.

Instead of explicitly calculating the length difference:

```text
curr1: A → B
curr2: B → A
```

Both pointers travel the same total distance:

```text
len(A) + len(B)
```

Therefore, if an intersection exists, they meet exactly at the intersection node.

---

### ⚙️ Algorithm

1. Set `curr1 = headA`, `curr2 = headB`.
2. While they are not equal:
   - If `curr1` reaches `nullptr`, move it to `headB`.
   - Otherwise move it to `curr1->next`.
   - If `curr2` reaches `nullptr`, move it to `headA`.
   - Otherwise move it to `curr2->next`.
3. Return `curr1`.

```cpp id="7c3p9m"
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* curr1 = headA;
        ListNode* curr2 = headB;

        while(curr1 != curr2) {
            curr1 = (curr1 == nullptr) ? headB : curr1->next;
            curr2 = (curr2 == nullptr) ? headA : curr2->next;
        }

        return curr1;
    }
};
```

---

### 🔍 Example

```text
A: 4 → 1 ─────→ 8 → 4 → 5
            ↗
B: 5 → 6 → 1
```

Conceptually:

```text
curr1: A → B
curr2: B → A
```

Eventually:

```text
curr1 = 8
curr2 = 8
```

Therefore `8` is returned.

---

### 🚫 No Intersection

If the lists don't intersect:

```text
A: 1 → 2 → 3 → NULL
B: 4 → 5 → NULL
```

Both pointers eventually become `nullptr`.

Since:

```cpp
curr1 == curr2
```

the loop terminates and `nullptr` is returned.

---

### ⚠️ Common Mistake

A subtle mistake is:

```cpp
if(curr1 == nullptr)
    curr1 = headB;

curr1 = curr1->next;  // ❌
```

This **skips `headB`**.

Correct:

```cpp
curr1 = (curr1 == nullptr) ? headB : curr1->next;
```

The same applies to `curr2`.

---

### 🧠 Why It Works

Let:

- `a` = unique portion of list A
- `b` = unique portion of list B
- `c` = common portion

Pointer 1 travels:

```text
A → B = a + c + b + c
```

Pointer 2 travels:

```text
B → A = b + c + a + c
```

Both distances are:

```text
a + b + 2c
```

So they arrive at the intersection together.

If there is no intersection, both reach `nullptr` after traversing both lists.

---

### ⏱️ Complexity

- **Time:** `O(n + m)`
- **Auxiliary Space:** `O(1)`
- **Optimal:** ✅

---

### 🎯 Pattern Recognition

When you see:

> **Two linked lists + find common/intersection node + O(1) space**

Think:

**Two pointers → switch heads at NULL.**

Don't calculate lengths unless you specifically want the alternative solution.

---

### 🔑 Core Insight

> **Make both pointers travel through both lists. This automatically removes the difference in their lengths.**
