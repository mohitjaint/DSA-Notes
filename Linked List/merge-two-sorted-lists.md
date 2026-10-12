## Merge Two Sorted Lists — LeetCode 21

### 🔗 References

- [LeetCode — Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
- Related: Merge K Sorted Lists
- Related: Sort List

---

### 🧩 Pattern

**Two Pointers + Sorted Merge**

Both linked lists are already sorted, so compare their current nodes and always take the smaller one.

```text id="w8x4zp"
List 1: 1 → 3 → 5
List 2: 2 → 4 → 6

          ↓ merge

Result:   1 → 2 → 3 → 4 → 5 → 6
```

---

### 💡 Key Observation

Because both lists are sorted:

> The smallest remaining element must be at the current node of either `list1` or `list2`.

Therefore, we don't need to look ahead or sort anything.

```cpp id="2q5n0f"
if(list1->val < list2->val)
```

Take the smaller node and advance that list's pointer.

---

### ⚙️ Algorithm

1. Create a **dummy node** to simplify constructing the result.
2. Maintain `curr` as the tail of the merged list.
3. While both lists contain nodes:
   - Compare their values.
   - Attach the smaller node to `curr`.
   - Advance that list.
   - Advance `curr`.
4. Once one list becomes empty, attach the remaining list directly.
5. Return `dummy->next`.

---

### 🧠 Code

```cpp id="g2b7s1"
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode* dummy = new ListNode(-1);
        ListNode* curr = dummy;

        while(list1 && list2) {
            if(list1->val < list2->val) {
                curr->next = list1;
                list1 = list1->next;
            }
            else {
                curr->next = list2;
                list2 = list2->next;
            }

            curr = curr->next;
        }

        curr->next = list1 ? list1 : list2;

        return dummy->next;
    }
};
```

---

### 🔍 Example

```text id="n7pq9x"
List 1:
1 → 3 → 5

List 2:
2 → 4 → 6
```

Compare:

```text id="wq0s7f"
1 < 2 → take 1
3 > 2 → take 2
3 < 4 → take 3
5 > 4 → take 4
5 < 6 → take 5
```

List 1 is now empty, so attach the remaining list:

```text id="y4z8n1"
1 → 2 → 3 → 4 → 5 → 6
```

---

### 🤔 Why can we directly attach the remaining list?

Suppose:

```text id="q3v6a8"
1 → 4 → 7
2 → 3
```

After processing:

```text id="v9q4cx"
1 → 2 → 3
```

The second list is exhausted.

The remaining first list is:

```text id="8n2m4p"
4 → 7
```

Since it was already sorted, **every remaining node is guaranteed to be ≥ everything already placed**.

So:

```cpp id="q7x5n2"
curr->next = list1;
```

is sufficient.

---

### ⚠️ Common Mistakes

#### 1. Creating new nodes unnecessarily

You don't need to create nodes for the merged list.

Reuse the existing nodes:

```cpp id="1a6v9z"
curr->next = list1;
```

This keeps auxiliary space `O(1)`.

---

#### 2. Forgetting to advance the selected list

After attaching:

```cpp id="8i3x0a"
curr->next = list1;
```

you must do:

```cpp id="3n9c2k"
list1 = list1->next;
```

Otherwise you'll repeatedly attach the same node.

---

#### 3. Forgetting to move `curr`

After every insertion:

```cpp id="r5n8q2"
curr = curr->next;
```

Otherwise the next node overwrites the previous connection.

---

#### 4. Traversing the remaining list manually

You can do:

```cpp
while(list1) { ... }
```

but it is unnecessary.

Just:

```cpp id="t4c7qm"
curr->next = list1 ? list1 : list2;
```

because the remaining list is already sorted.

---

### 🎯 Pattern Recognition

Whenever you see:

> **Two already sorted sequences/lists need to be combined**

Think:

**Two pointers → compare current elements → take smaller → advance.**

This exact pattern extends to:

- Merge K Sorted Lists
- Merge Sort
- Merging sorted arrays
- External sorting

---

### ⏱️ Complexity

Let:

- `n` = length of list 1
- `m` = length of list 2

**Time:**

```text
O(n + m)
```

Every node is visited at most once.

**Auxiliary Space:**

```text
O(1)
```

Only pointers are used. The dummy node is constant space.

### 🔑 Core Insight

> **When both inputs are sorted, never sort again. Compare their current elements and consume the smaller one.**

The **dummy node + two-pointer merge** is the standard pattern to memorize.
