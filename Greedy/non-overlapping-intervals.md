# Non-overlapping Intervals — LeetCode 435

## Reference

**Question:** [https://leetcode.com/problems/non-overlapping-intervals/](https://leetcode.com/problems/non-overlapping-intervals/)

**Pattern:**
`Greedy + Interval Scheduling`

**Related Problem:**
N Meetings in One Room / Activity Selection

---

# Problem

Given a collection of intervals, remove the **minimum number of intervals** so that the remaining intervals do not overlap.

---

# Key Observation

Instead of directly thinking:

> Which intervals should I remove?

Think:

> **What is the maximum number of non-overlapping intervals I can keep?**

Then:

```text id="3u9s1n"
removed = total intervals - maximum intervals kept
```

This transforms the problem into the classic **Activity Selection** problem.

---

# Greedy Strategy

Sort intervals by their **ending time**:

```cpp id="l5p6zx"
sort(intervals.begin(), intervals.end(),
    [](vector<int>& a, vector<int>& b) {
        return a[1] < b[1];
    });
```

Then greedily keep the interval that finishes earliest and is compatible with the previously selected interval.

### Why earliest finish?

An interval that finishes earlier leaves more room for future intervals.

So:

```text id="x7m2qa"
Earliest finishing interval
        ↓
Maximum remaining space
        ↓
Maximum number of intervals can be kept
```

This is exactly the same greedy principle as **Activity Selection / N Meetings in One Room**.

---

# Algorithm

Maintain:

```cpp id="b4j6ps"
int prevEnd = INT_MIN;
int toKeep = 0;
```

For each interval:

### Compatible

If:

```cpp id="9s8x1d"
interval[0] >= prevEnd
```

then the interval doesn't overlap the previously selected interval.

Keep it:

```cpp id="w2p7cn"
toKeep++;
prevEnd = interval[1];
```

### Overlapping

If:

```text id="5x9m2a"
interval[0] < prevEnd
```

then it overlaps the previously selected interval.

Skip it.

Because intervals are sorted by end time, the previously selected interval is guaranteed to have the earliest possible ending time.

---

# Example

```text id="x4g8vs"
intervals =
[[1,2], [2,3], [3,4], [1,3]]
```

Sort by end time:

```text id="k6y1mz"
[1,2]
[2,3]
[1,3]
[3,4]
```

### `[1,2]`

```text id="r7m3qx"
1 >= INT_MIN
```

Keep.

```text id="v5p8nc"
prevEnd = 2
toKeep = 1
```

### `[2,3]`

```text id="z2x6ka"
2 >= 2
```

Keep.

```text id="g9w4mt"
prevEnd = 3
toKeep = 2
```

### `[1,3]`

```text id="q6n8vs"
1 < 3
```

Overlap → skip.

### `[3,4]`

```text id="m1r7zp"
3 >= 3
```

Keep.

Final:

```text id="c8x2nd"
toKeep = 3
```

There are `4` intervals:

```text id="4n7qpx"
removed = 4 - 3 = 1
```

---

# Code

```cpp id="5p8w2c"
class Solution {
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {

        int toKeep = 0;
        int prevEnd = INT_MIN;

        sort(intervals.begin(), intervals.end(),
            [](vector<int>& a, vector<int>& b) {
                return a[1] < b[1];
            }
        );

        for(auto interval : intervals) {
            if(interval[0] >= prevEnd) {
                toKeep++;
                prevEnd = interval[1];
            }
        }

        return intervals.size() - toKeep;
    }
};
```

---

# Why `>=`?

For this problem, intervals that only touch are **not considered overlapping**.

For example:

```text id="x3q7nm"
[1,2]
[2,3]
```

These can both be kept.

Therefore:

```cpp id="j4k8pw"
start >= prevEnd
```

is valid.

---

# Why Sorting by Start Time Doesn't Work

Consider:

```text id="q8m3vx"
[1,10]
[2,3]
[3,4]
```

If we sort by start time:

```text id="x4n7cp"
[1,10]
[2,3]
[3,4]
```

we might select `[1,10]`, which blocks both other intervals.

But sorting by end time gives:

```text id="a9v2ks"
[2,3]
[3,4]
[1,10]
```

We keep:

```text id="7p3xnm"
[2,3]
[3,4]
```

and remove only `[1,10]`.

The earliest-ending choice is therefore the correct greedy decision.

---

# Why the Greedy Strategy Is Optimal

Suppose several intervals are available.

Choose:

```text id="2v8mqa"
A = earliest finishing interval
```

Any other available interval `B` finishes at the same time or later:

```text id="q6x1kp"
end(A) <= end(B)
```

Therefore, choosing `A` leaves at least as much room for every future interval as choosing `B`.

So replacing any other first choice with the earliest-ending interval cannot reduce the maximum number of intervals we can keep.

This is the **exchange argument** behind Activity Selection.

---

# Common Mistakes

### 1. Trying to minimize removals directly

It's easier to think:

```text id="x7n2qm"
maximize intervals kept
```

and then:

```text id="f4p8cz"
removed = n - kept
```

---

### 2. Sorting by start time

The correct greedy criterion is:

```text id="r9m3kx"
earliest ending time
```

not earliest starting time.

---

### 3. Using `>` instead of `>=`

```cpp id="v8q1ny"
interval[0] >= prevEnd
```

is required because touching intervals are allowed.

---

### 4. Updating `prevEnd` when skipping

Suppose:

```text id="2f7mxa"
previous selected = [1,3]
current = [2,4]
```

The current interval overlaps, so skip it.

Do **not** do:

```cpp
prevEnd = interval[1];
```

The selected interval `[1,3]` still determines the boundary.

---

# Complexity

Sorting:

```text id="y4m8qs"
O(n log n)
```

Greedy scan:

```text id="k7p2zx"
O(n)
```

Overall:

```text id="q3v9mc"
Time  : O(n log n)
Space : O(1) auxiliary
```

The input is sorted in place.

---

# Pattern Recognition

When you see:

```text id="a8k2mq"
Intervals
+
Maximum number of non-overlapping intervals
```

think:

```text id="j5v7nx"
Activity Selection
        ↓
Sort by end time
        ↓
Keep earliest finishing compatible interval
```

If the problem instead asks:

```text id="p3x9kw"
Minimum intervals to remove
```

convert it:

```text id="u7m4qc"
Minimum removed
=
Total intervals
-
Maximum intervals that can be kept
```

---

# Connection to N Meetings in One Room

These two problems use essentially the **same greedy algorithm**.

### N Meetings

```text id="f8q2mz"
Goal:
maximize meetings selected

Strategy:
sort by finish time
```

### Non-overlapping Intervals

```text id="k3m7xp"
Goal:
minimize intervals removed

Convert to:
maximize intervals kept

Strategy:
sort by finish time
```

So when you solve one, you should immediately recognize the other.

---

# Core Insight

> **To remove the minimum number of overlapping intervals, keep the maximum number of mutually non-overlapping intervals. The maximum set is obtained greedily by always choosing the compatible interval with the earliest finishing time.**

The pattern:

```text id="r4x8np"
Minimum removals
      ↓
Maximum intervals kept
      ↓
Activity Selection
      ↓
Sort by end time
      ↓
Keep earliest finishing compatible interval
      ↓
Answer = n - kept
```

**Complexity: `O(n log n)` time, `O(1)` auxiliary space.**
