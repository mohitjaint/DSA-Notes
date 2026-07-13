# Minimum Window Subsequence (GeeksforGeeks)

## References

**Question:**

[https://www.geeksforgeeks.org/problems/minimum-window-subsequence/1](https://www.geeksforgeeks.org/problems/minimum-window-subsequence/1)

**Related Problems:**

- Minimum Window Substring (contrast)
- Is Subsequence
- Distinct Subsequences
- Two Pointer Greedy

---

# Pattern

```text
Greedy

Forward + Backward Scan

Two Pointers
```

> **Not a classical sliding window problem.**

---

# Problem Statement

Given two strings

```text
S1
```

and

```text
S2
```

find the **smallest substring of S1** such that **S2 is a subsequence** of that substring.

---

# Difference from Minimum Window Substring

### Minimum Window Substring

Need to contain

```text
all characters
```

Order does **not** matter.

Example

```text
ABC

BAC
```

Valid.

---

### Minimum Window Subsequence

Need to contain

```text
characters in the same order
```

Example

```text
ABC
```

Valid.

```text
BAC
```

Invalid.

---

# Why Normal Sliding Window Fails

Sliding window works when validity depends only on

```text
frequency
```

Here validity depends on

```text
relative order
```

When removing the leftmost character,

we cannot determine in O(1) whether another suitable occurrence later in the window can still preserve the subsequence.

Therefore,

```text
Expand → Shrink
```

does not work.

---

# Key Observation

Once a complete subsequence is found,

the **right boundary is fixed**.

The only thing left is to minimize the **left boundary**.

This can be done by scanning backwards.

---

# Core Idea

The algorithm consists of two phases.

---

## Phase 1 — Forward Scan

Move right pointer through

```text
S1
```

Maintain an index

```cpp
it
```

into

```text
S2
```

Whenever

```cpp
S1[r] == S2[it]
```

advance

```cpp
it++
```

When

```cpp
it == S2.length()
```

a valid window has been found.

---

## Phase 2 — Backward Scan

Start from the current right boundary.

Move left.

Whenever

```cpp
S1[l] == S2[it-1]
```

move backwards in

```text
S2
```

until

```cpp
it == 0
```

At this point,

the smallest possible starting position for this right boundary has been found.

---

# Update Answer

Current window

```text
[l+1 .... r]
```

Length

```text
r-l
```

Update the minimum answer if smaller.

---

# Restart

Instead of continuing from the current right boundary,

restart from

```cpp
r = l + 1;
```

Then continue the forward scan.

This avoids unnecessary rescanning while still considering overlapping windows.

---

# Complete Algorithm

```text
Forward scan

↓

Match complete subsequence

↓

Backward scan

↓

Find minimum start

↓

Update answer

↓

Restart from new start

↓

Repeat
```

---

# Complexity

```text
Time : O(n × m) worst case

Space : O(1)
```

Although the backward scan may revisit characters, the constraints are small enough for this approach to pass comfortably.

---

# Why It Works

The forward scan guarantees that

```text
S2
```

appears as a subsequence.

The backward scan greedily removes unnecessary characters from the left while preserving the subsequence.

Therefore,

for every discovered right boundary,

the algorithm computes the **minimum possible window** ending there.

Taking the minimum over all such windows yields the answer.

---

# Common Mistake #1

Trying to apply the Minimum Window Substring template.

```text
Expand

↓

Shrink while valid
```

This fails because validity depends on subsequence order rather than frequencies.

---

# Common Mistake #2

Thinking only frequencies matter.

Example

```text
Window = BAC

Target = ABC
```

All characters exist,

but

```text
ABC
```

is **not** a subsequence.

---

# Common Mistake #3

Stopping after the first valid window.

A later window may be shorter.

Always continue searching.

---

# Common Mistake #4

Off-by-one during backward scan.

After finishing the backward scan,

the actual start becomes

```text
l + 1
```

because

```cpp
l
```

has already moved one position too far.

---

# Comparison

| Problem                    | Condition                     | Technique                 |
| -------------------------- | ----------------------------- | ------------------------- |
| Minimum Window Substring   | Contains all characters       | Sliding Window            |
| Minimum Window Subsequence | Contains subsequence in order | Forward + Backward Greedy |

---

# Pattern Recognition

Whenever the problem asks for

```text
Minimum substring

+

Another string must appear as a subsequence
```

think

```text
Forward scan

↓

Find subsequence

↓

Backward scan

↓

Minimize window

↓

Repeat
```

Do **not** try to maintain a sliding window invariant.

---

# Interview Trigger

```text
Minimum Window

↓

Subsequence (not substring)

↓

Order matters

↓

Sliding Window fails

↓

Forward matching

↓

Backward shrinking

↓

Greedy

↓

O(n × m)
```

---

# Core Insight

The defining property of this problem is that **window validity depends on the order of characters, not just their frequencies**. This breaks the classical sliding window template. Instead, use a **forward scan** to locate the end of a valid subsequence, then a **backward scan** to greedily remove unnecessary characters and obtain the smallest window ending at that position. Repeating this process over the string guarantees the minimum valid window.
