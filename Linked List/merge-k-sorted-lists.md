# Merge K Sorted Lists — LeetCode 23

## 🔗 References

- [LeetCode — Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)
- Related: [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
- Related: [Sort List](https://leetcode.com/problems/sort-list/)

## 🧩 Pattern

Min-Heap / Priority Queue + K-Way Merge

Merge multiple sorted linked lists by repeatedly selecting the smallest among their current nodes.

## 💡 Key Observation

Suppose we have:

```
List 1: 1 → 4 → 7
List 2: 2 → 5 → 8
List 3: 3 → 6 → 9
```

Because every list is sorted, the smallest remaining element must be at the head of one of the lists.

Instead of scanning all \\(k\\) heads for every node, maintain them in a min-heap.

- `pq.top()` gives the smallest current node.
- After extracting it, insert its successor from the same list.
- Repeat until the heap is empty.

Invariant: The heap contains the first unprocessed node of every non-empty list.

## ⚙️ Algorithm

1. Create a min-heap of `ListNode*`, ordered by node value.
2. Insert the head of every non-empty list.
3. Create a dummy node and a `curr` pointer for building the result.
4. While the heap isn't empty:
   - Extract the smallest node.
   - Attach it to the merged list.
   - Insert its `next` node if it exists.
5. Terminate the final node and return `dummy.next`.

## 💻 Optimal Heap Solution

```
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        auto cmp = [](ListNode* a, ListNode* b) {
            return a->val > b->val;
        };

        priority_queue<
            ListNode*,
            vector<ListNode*>,
            decltype(cmp)
        > pq(cmp);

        for (auto listHead : lists) {
            if (listHead)
                pq.push(listHead);
        }

        ListNode dummy(0);
        ListNode* curr = &dummy;

        while (!pq.empty()) {
            ListNode* smallest = pq.top();
            pq.pop();

            curr->next = smallest;
            curr = curr->next;

            if (smallest->next)
                pq.push(smallest->next);
        }

        curr->next = nullptr;
        return dummy.next;
    }
};
```

## 🔍 Example Walkthrough

Input:

```
List 1: 1 → 4 → 5
List 2: 1 → 3 → 4
List 3: 2 → 6
```

Initially, the heap contains:

```
1, 1, 2
```

| Step | Extract minimum | Insert successor | Merged result   |
| ---- | --------------- | ---------------- | --------------- |
| 1    | 1               | 4                | `1`             |
| 2    | 1               | 3                | `1 → 1`         |
| 3    | 2               | 6                | `1 → 1 → 2`     |
| 4    | 3               | 4                | `1 → 1 → 2 → 3` |
| 5    | 4               | 5                | `... → 4`       |
| 6    | 4               | None             | `... → 4`       |
| 7    | 5               | None             | `... → 5`       |
| 8    | 6               | None             | `... → 6`       |

Output:

```
1 → 1 → 2 → 3 → 4 → 4 → 5 → 6
```

## 🧠 Why It Works

At every step, the heap contains the smallest unprocessed node from each non-empty list.

Since each list is sorted, all subsequent nodes in a list are at least as large as its current node. Therefore, the minimum across the heap is also the globally smallest unprocessed node.

Extracting it and inserting its successor preserves this property. Repeating the process produces the fully sorted merged list.

## ⚠️ Common Mistakes

1\. Reversing the comparator

```
return a->val > b->val;
```

This creates a min-heap. Using `<` instead creates a max-heap.

2\. Forgetting to insert the successor

```
if (smallest->next)
    pq.push(smallest->next);
```

Without this, only the initial head nodes would be processed.

3\. Inserting null pointers

Only insert non-empty list heads and non-null successors.

4\. Creating unnecessary result nodes

```
curr->next = smallest;
```

Reuses the original nodes rather than allocating a new node for every element.

5\. Forgetting the final termination

```
curr->next = nullptr;
```

Explicitly terminates the merged list when reusing nodes.

## ⏱️ Complexity Analysis

Let \\(N\\) be the total number of nodes and \\(k\\) the number of lists.

- Time: \\(O(N\log k)\\)
- Auxiliary space: \\(O(k)\\) for the heap.
- Output nodes: Existing nodes are reused; only the stack-allocated dummy node is additional.

Each node is inserted into and removed from the heap at most once, and each heap operation costs \\(O(\log k)\\).

## 🔄 Alternative Approach: Divide and Conquer

Another standard solution repeatedly merges pairs of lists using the `mergeTwoLists` function.

- Time: \\(O(N\log k)\\)
- Auxiliary space: Depends on the implementation; iterative pairwise merging can use \\(O(1)\\) auxiliary space beyond the list storage.

Both approaches have the same asymptotic time complexity. The heap approach is convenient when you want to repeatedly extract the smallest current element.

## 🎯 Pattern Recognition

When you see:

> K sorted lists or arrays that need to be merged efficiently

Think:

Min-heap → extract smallest → insert successor.

This pattern also appears in:

- Merging K sorted arrays
- Finding the Kth smallest element across sorted lists
- K-way merging in external sorting
- Merging sorted data streams

## 🔑 Core Insight

Keep only one unprocessed candidate from each list in the min-heap. Because each list is sorted, processing the smallest candidate and adding its successor is enough to maintain global sorted order.
