# Merge Intervals — LeetCode 56

## Reference

**Question:** [https://leetcode.com/problems/merge-intervals/](https://leetcode.com/problems/merge-intervals/)

**Pattern:**
`Intervals + Sorting + Greedy`

**Related Problems:**

- Insert Interval — LeetCode 57
- Non-overlapping Intervals — LeetCode 435
- Meeting Rooms II

---

# Problem

Given a collection of intervals:

```text
[start, end]
```

merge all overlapping intervals.

The final answer should contain **non-overlapping intervals** covering the same ranges as the original intervals.

---

# Key Observation

The intervals are **not necessarily sorted**.

Therefore, first sort them by their starting point:

```cpp
sort(intervals.begin(), intervals.end());
```

After sorting:

```text id="9o7p2v"
[start₁, end₁]
[start₂, end₂]
[start₃, end₃]
...
```

Now all intervals that could overlap the current interval appear immediately after it.

This reduces the problem to a simple scan.

---

# Main Greedy Idea

Keep the **last merged interval**.

For every new interval:

### No overlap

If:

```cpp
ans.back()[1] < interval[0]
```

then:

```text id="h3q8vx"
merged interval: [1,3]
current:                 [5,7]
```

There is a gap.

So the current interval starts a new merged interval.

```cpp
ans.push_back(interval);
```

---

### Overlap

Otherwise:

```cpp
ans.back()[1] >= interval[0]
```

so the intervals overlap.

Merge them by extending the end:

```cpp
ans.back()[1] =
    max(ans.back()[1], interval[1]);
```

We don't need to change the start because the intervals are sorted by start time.

---

# The Clean Solution

```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end());

        vector<vector<int>> ans;

        for(auto &interval : intervals) {
            if(ans.empty() || ans.back()[1] < interval[0]) {
                ans.push_back(interval);
            }
            else {
                ans.back()[1] =
                    max(ans.back()[1], interval[1]);
            }
        }

        return ans;
    }
};
```

---

# Example

Consider:

```text
intervals =
[[1,3], [2,6], [8,10], [9,12]]
```

### Step 1 — Sort

Already sorted:

```text
[1,3]
[2,6]
[8,10]
[9,12]
```

### Step 2 — Process

Start:

```text
ans = []
```

Take:

```text
[1,3]
```

```text
ans = [[1,3]]
```

---

Next:

```text
[2,6]
```

Check:

```text
ans.back()[1] = 3
interval[0] = 2

3 >= 2
```

Overlap.

Merge:

```text
[1,6]
```

---

Next:

```text
[8,10]
```

Check:

```text
6 < 8
```

No overlap.

Add:

```text
[[1,6], [8,10]]
```

---

Next:

```text
[9,12]
```

Check:

```text
10 >= 9
```

Overlap.

Merge:

```text
[8,12]
```

Final:

```text
[[1,6], [8,12]]
```

---

# Why Sorting Is Important

Without sorting, consider:

```text
[8,10]
[1,5]
[2,6]
```

The intervals `[1,5]` and `[2,6]` overlap, but they aren't next to each other.

After sorting:

```text
[1,5]
[2,6]
[8,10]
```

all potentially overlapping intervals become adjacent.

So sorting converts the problem into a simple linear scan.

---

# Why `ans.back()` Works

After processing some intervals:

```text
ans = [
    [1,6],
    [8,10]
]
```

The last interval:

```cpp
ans.back()
```

is always the only interval that can possibly overlap with the next interval.

Why?

Because everything before it has already been finalized and is strictly separated from the current range.

Therefore we don't need to compare the new interval against **every** interval in `ans`.

---

# Important Detail: Don't Use `min()` for the End

Suppose:

```text
[1,5]
[2,10]
```

The merged interval must be:

```text
[1,10]
```

so:

```cpp
ans.back()[1] = max(
    ans.back()[1],
    interval[1]
);
```

not:

```cpp
min(...)
```

---

# Your Two-Pointer Approach

Your solution uses a slightly different representation:

```text
i → current interval being merged
j → next interval
```

The important logic was:

```cpp
if(intervals[j][0] > intervals[i][1]) {
    ans.push_back(intervals[i]);
    i = j;
}
else {
    intervals[i][1] =
        max(intervals[i][1], intervals[j][1]);
}
```

This is completely valid.

### Mental model

```text
i = current merged interval
j = interval we're examining

          ↓

Does j overlap i?

     YES              NO
      ↓                ↓
   merge j          finalize i
      ↓                ↓
   j++               i = j
```

Your approach modifies `intervals[i]` in place, whereas the cleaner version maintains the current merged interval inside `ans`.

---

# Comparison

|                | Your Approach | `ans.back()` Approach |
| -------------- | ------------- | --------------------- |
| Sort           | Yes           | Yes                   |
| Greedy scan    | Yes           | Yes                   |
| Time           | O(n log n)    | O(n log n)            |
| Output space   | O(n)          | O(n)                  |
| Modifies input | **Yes**       | No                    |
| Extra pointers | `i`, `j`      | Only iteration        |
| Readability    | Good          | **Cleaner**           |

The underlying algorithm is **exactly the same**.

---

# Complexity

### Sorting

```text
O(n log n)
```

### Single scan

```text
O(n)
```

Therefore:

```text
Time: O(n log n)
```

Space:

```text
O(n)
```

for the output.

Ignoring the output:

```text
Auxiliary space: O(1)
```

assuming the sorting implementation's stack space is ignored.

---

# Common Mistakes

### 1. Forgetting to sort

Without sorting, the linear merge strategy doesn't work.

---

### 2. Comparing only starts

This is not enough.

You need to compare:

```text
current interval's end
        vs
next interval's start
```

Specifically:

```cpp
ans.back()[1] < interval[0]
```

means **no overlap**.

---

### 3. Forgetting to extend the end

For:

```text
[1,5]
[2,10]
```

the result isn't `[1,5]`.

It must become:

```text
[1,10]
```

Hence:

```cpp
max(end1, end2)
```

---

### 4. Comparing against every previous interval

Once an interval has been merged into `ans`, only the **last merged interval** can overlap with the next interval.

That's why:

```cpp
ans.back()
```

is enough.

---

# Pattern Recognition

When you see:

```text
Intervals
+
Need to combine overlapping ranges
```

think:

```text
Sort by start
      ↓
Keep current merged interval
      ↓
Overlap?
 ┌────┴────┐
YES        NO
 ↓          ↓
Merge     Start new interval
```

This is the fundamental **Merge Intervals** pattern.

---

# Merge Intervals vs Insert Interval

These two problems are closely related.

### Merge Intervals

Input is unsorted:

```text
[8,10] [1,3] [2,6]
```

Therefore:

```text
Sort first
    ↓
Merge
```

Complexity:

```text
O(n log n)
```

### Insert Interval

Input is **already sorted and non-overlapping**:

```text
[1,3] [6,9]
```

Therefore:

```text
No sorting
    ↓
Before → Overlap → After
```

Complexity:

```text
O(n)
```

This distinction is worth remembering.

---

# Core Insight

> **Sort intervals by their start time so that all potentially overlapping intervals become adjacent. Then keep only the last merged interval: if the next interval starts after its end, start a new interval; otherwise extend the current end.**

The entire pattern:

```text
Unsorted intervals
       ↓
Sort by start
       ↓
Current merged interval
       ↓
Overlap?
  ↙          ↘
YES          NO
 ↓            ↓
Extend       Finalize
end          + new interval
```

**Time: `O(n log n)` — standard optimal solution.**
