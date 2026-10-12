# Sort List — LeetCode 148

## 🔗 References

- [LeetCode — Sort List](https://leetcode.com/problems/sort-list/)
- Related: [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
- Related: [Merge K Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

## 🧩 Pattern

Divide and Conquer + Merge Sort on Linked Lists

Split the linked list into two halves, recursively sort each half, and merge the sorted halves.

## 💡 Key Observation

Merge sort works particularly well for linked lists because we can split and merge them using pointers without shifting elements.

For example:

```
4 → 2 → 1 → 3
```

Split into two halves:

```
4 → 2       1 → 3
```

Recursively sort each half:

```
2 → 4       1 → 3
```

Merge the sorted halves:

```
1 → 2 → 3 → 4
```

The key is that merging two sorted lists takes linear time, and there are \\(O(\log n)\\) levels of division.

## ⚙️ Algorithm

1. Base case: If the list is empty or contains one node, return it.
2. Find the middle using slow and fast pointers.
3. Split the list into two independent halves by setting `middle->next = nullptr`.
4. Recursively sort both halves.
5. Merge the sorted halves using the standard two-pointer merge algorithm.
6. Return the merged head.

## 💻 Code — Recursive Merge Sort

```
class Solution {
public:
    ListNode* findMiddle(ListNode* head) {
        ListNode* slow = head;
        ListNode* fast = head->next;

        while (fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
        }

        return slow;
    }

    ListNode* merge(ListNode* a, ListNode* b) {
        ListNode dummy(-1);
        ListNode* curr = &dummy;

        while (a && b) {
            if (a->val < b->val) {
                curr->next = a;
                a = a->next;
            } else {
                curr->next = b;
                b = b->next;
            }

            curr = curr->next;
        }

        curr->next = a ? a : b;

        return dummy.next;
    }

    ListNode* sortList(ListNode* head) {
        if (!head || !head->next)
            return head;

        ListNode* middle = findMiddle(head);

        ListNode* leftHead = head;
        ListNode* rightHead = middle->next;

        // Split into independent lists
        middle->next = nullptr;

        leftHead = sortList(leftHead);
        rightHead = sortList(rightHead);

        return merge(leftHead, rightHead);
    }
};
```

## 🔍 Why Start `fast` at `head->next`?

```
ListNode* slow = head;
ListNode* fast = head->next;
```

This makes `slow` stop at the last node of the first half when the list has an even number of nodes.

For example:

```
1 → 2 → 3 → 4
    ↑
   slow
```

Then:

```
ListNode* rightHead = middle->next;
middle->next = nullptr;
```

produces:

```
1 → 2       3 → 4
```

Both halves are strictly smaller than the original list, ensuring the recursion progresses toward the base case.

## 🧠 Why It Works

The correctness follows from two facts:

1. Recursively sorting the two halves produces two sorted lists.
2. Merging two sorted lists produces one sorted list containing all their nodes.

By induction, the base case of a one-node list is already sorted. Each recursive merge then produces a correctly sorted larger list.

## ⚠️ Common Mistakes

- Forgetting to split the list: Without `middle->next = nullptr`, recursive calls may keep processing the same list.
- Finding the wrong middle: Starting `fast` at `head->next` helps ensure the first half is correctly separated.
- Forgetting to advance `curr` during merging: This can overwrite links and lose nodes.
- Creating unnecessary nodes: Reuse the existing nodes during merging.
- Forgetting the remainder: When one list becomes empty, attach the other list directly:
  ```
  curr->next = a ? a : b;
  ```

## 🎯 Pattern Recognition

When you see:

> Sort a linked list in \\(O(n\log n)\\) time

Think:

Find middle → split → recursively sort → merge.

The same merge operation used in [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/) is the foundation of this solution.

## ⏱️ Complexity

- Time: \\(O(n\log n)\\)
- Auxiliary space: \\(O(\log n)\\) for the recursion stack.
- New data nodes: None; the existing nodes are reused.

The recursive solution meets the main time-complexity requirement. The iterative bottom-up variant can achieve \\(O(1)\\) auxiliary space, but you don't need to master it before moving on.

## 🔑 Core Insight

Merge sort is effective on linked lists because splitting and merging can be performed by changing pointers, without shifting elements.

Remember the three essential operations:

1. Find the midpoint.
2. Break the list into two halves.
3. Merge the recursively sorted halves.
