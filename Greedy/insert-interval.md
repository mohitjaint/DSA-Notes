# Insert Interval — LeetCode 57

## Reference

**Question:** [https://leetcode.com/problems/insert-interval/](https://leetcode.com/problems/insert-interval/)

**Pattern:**
`Intervals + Greedy + Two Pointers`

---

# Problem

Given a sorted list of **non-overlapping intervals** and a `newInterval`, insert the new interval while maintaining:

- Sorted order
- No overlapping intervals

If the new interval overlaps existing intervals, merge them.

---

# Key Observation

Because the existing intervals are already:

```text
sorted by start time
+
non-overlapping
```

we don't need to compare every possible endpoint configuration.

Every existing interval falls into exactly **one of three categories**:

```text
BEFORE → OVERLAP → AFTER
```

---

# Case 1 — Completely Before

If:

```cpp
intervals[i][1] < newInterval[0]
```

then:

```text
existing interval:
[-----]

new interval:
        [-----]
```

The existing interval finishes before the new interval starts.

So simply add it:

```cpp
ans.push_back(intervals[i]);
```

---

# Case 2 — Overlapping

If the interval is neither completely before nor completely after, it overlaps.

The overlap condition can be written as:

```cpp
intervals[i][0] <= newInterval[1]
```

while we are already past all intervals satisfying:

```cpp
intervals[i][1] < newInterval[0]
```

Merge:

```cpp
newInterval[0] =
    min(newInterval[0], intervals[i][0]);

newInterval[1] =
    max(newInterval[1], intervals[i][1]);
```

This handles **all possible overlap configurations**.

For example:

```text
[3,5] with [4,8]
[3,8] with [4,6]
[4,8] with [3,5]
[3,10] with [5,7]
```

All are handled by the same:

```cpp
min(start)
max(end)
```

---

# Case 3 — Completely After

If:

```cpp
intervals[i][0] > newInterval[1]
```

then:

```text
new interval:
[-----]

existing:
        [-----]
```

The new interval belongs before this interval.

At this point, because the intervals are sorted, **every remaining interval is also after the new interval**.

So:

1. Add `newInterval`
2. Append all remaining intervals

---

# Algorithm

```text
i = 0

1. Add all intervals completely before newInterval

        ↓

2. Merge all overlapping intervals into newInterval

        ↓

3. Add merged newInterval

        ↓

4. Add all remaining intervals
```

The resulting structure is:

```text
BEFORE
  ↓
OVERLAPPING
  ↓
MERGED NEW INTERVAL
  ↓
AFTER
```

---

# Code

```cpp
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals,
                                vector<int>& newInterval) {

        vector<vector<int>> ans;
        int n = intervals.size();
        int i = 0;

        // Intervals completely before newInterval
        while(i < n && intervals[i][1] < newInterval[0]){
            ans.push_back(intervals[i]);
            i++;
        }

        // Merge overlapping intervals
        while(i < n && intervals[i][0] <= newInterval[1]){
            newInterval[0] =
                min(newInterval[0], intervals[i][0]);

            newInterval[1] =
                max(newInterval[1], intervals[i][1]);

            i++;
        }

        // Add merged interval
        ans.push_back(newInterval);

        // Remaining intervals
        while(i < n){
            ans.push_back(intervals[i]);
            i++;
        }

        return ans;
    }
};
```

---

# Example

```text
intervals = [[1,3], [6,9]]
newInterval = [2,5]
```

### Before

Check:

```text
[1,3]
```

Since:

```text
3 < 2  ❌
```

it is not completely before.

### Overlap

```text
[1,3] overlaps [2,5]
```

Merge:

```text
[1,5]
```

Next interval:

```text
[6,9]
```

Since:

```text
6 > 5
```

it is completely after.

Final:

```text
[[1,5], [6,9]]
```

---

# Why No Sorting Is Needed

This is important.

The input intervals are already:

```text
sorted
+
non-overlapping
```

Therefore, after finding the first interval after `newInterval`, we know that **all subsequent intervals are also after it**.

That's why the entire problem can be solved in:

```text
O(n)
```

instead of sorting.

---

# Complexity

Each interval is processed at most once.

```text
Time  : O(n)
Space : O(n)
```

The `O(n)` space is primarily for the output array.

If output space is excluded:

```text
Auxiliary Space: O(1)
```

---

# Common Mistakes

### 1. Enumerating too many overlap cases

You don't need separate cases for:

```text
new interval inside existing
existing interval inside new
partial overlap from left
partial overlap from right
```

All of them are simply:

```text
OVERLAP
```

and can be handled using:

```cpp
newStart = min(newStart, start);
newEnd = max(newEnd, end);
```

---

### 2. Forgetting that intervals are sorted

Once:

```cpp
intervals[i][0] > newInterval[1]
```

is found, you don't need to check whether later intervals overlap.

They cannot, because their starting points are even larger.

---

### 3. Adding `newInterval` too early

You need to finish merging **all overlapping intervals** before inserting the final merged interval.

---

### 4. Using the wrong overlap boundaries

The clean three-phase conditions are:

```cpp
// Before
intervals[i][1] < newInterval[0]

// Overlap
intervals[i][0] <= newInterval[1]

// After
intervals[i][0] > newInterval[1]
```

---

# Pattern Recognition

When you see:

```text
Sorted intervals
+
Non-overlapping intervals
+
Insert/merge one interval
```

think:

```text
BEFORE
   ↓
OVERLAP
   ↓
AFTER
```

No sorting is necessary.

---

# Interval Pattern

This problem is an important variation of **Merge Intervals**.

### Merge Intervals

```text
Sort all intervals
        ↓
Merge overlaps
```

### Insert Interval

```text
Input already sorted
        ↓
Find BEFORE region
        ↓
Merge OVERLAP region
        ↓
Append AFTER region
```

---

# Core Insight

> **Because the intervals are already sorted and non-overlapping, there are only three regions relative to the new interval: intervals completely before it, intervals overlapping it, and intervals completely after it.**

So the entire solution reduces to:

```text
Before:
    copy

Overlap:
    expand newInterval

After:
    copy
```

This gives the optimal **O(n)** solution.
