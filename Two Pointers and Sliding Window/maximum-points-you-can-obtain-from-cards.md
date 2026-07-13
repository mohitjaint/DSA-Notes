# Maximum Points You Can Obtain from Cards (LeetCode 1423)

## References

**Question:**

[https://leetcode.com/problems/maximum-points-you-can-obtain-from-cards/](https://leetcode.com/problems/maximum-points-you-can-obtain-from-cards/)

**Related Problems:**

- Maximum Sum Circular Subarray
- Sliding Window Maximum
- Minimum Size Subarray Sum
- Pick from Ends DP (contrast)

---

# Pattern

```text
Two Pointers

Prefix + Suffix Enumeration

Greedy Enumeration
```

---

# Problem Statement

You are given an array of card points.

You must pick exactly

```text
k
```

cards.

Each pick can only be made from

- the left end
- the right end

Return the maximum score.

---

# Brute Force

Try every sequence of left/right picks.

Since each pick has two choices,

```text
Time : O(2^k)
```

Not feasible.

---

# Key Observation

Suppose we pick

```text
x cards from left
```

Then we must pick

```text
k - x cards from right
```

where

```text
x = 0,1,2,...,k
```

There are only

```text
k+1
```

possible combinations.

---

# Core Idea

Start by taking

```text
all k cards from the left.
```

Then gradually replace one left card with one right card.

This generates every possible valid selection.

---

# Enumeration Order

For example,

```text
k = 4
```

Possible selections become

```text
LLLL

LLLR

LLRR

LRRR

RRRR
```

Every valid combination is visited exactly once.

---

# Variables

```cpp
lsum
```

Sum of selected cards from the left.

```cpp
rsum
```

Sum of selected cards from the right.

```cpp
maxSum
```

Best answer seen so far.

---

# Initialization

Take all cards from the left.

```cpp
for(int i=0;i<k;i++)
    lsum += cardPoints[i];

maxSum = lsum;
```

---

# Transition

Remove one card from the left.

```cpp
lsum -= cardPoints[l];
```

Add one card from the right.

```cpp
rsum += cardPoints[r];
```

Update answer.

```cpp
maxSum = max(maxSum, lsum + rsum);
```

Repeat until no left cards remain.

---

# Complete Algorithm

```text
Take first k cards

↓

Answer = left sum

↓

Move one card

Left → Removed

Right → Added

↓

Update answer

↓

Repeat k times
```

---

# Complexity

```text
Time : O(k)

Space : O(1)
```

---

# Why It Works

Every valid selection consists of

```text
Left Picks

+

Right Picks
```

such that

```text
Left Picks + Right Picks = k
```

Since we enumerate

```text
(k,0)

(k−1,1)

(k−2,2)

...

(0,k)
```

every possible answer is checked exactly once.

---

# Alternative Observation

Instead of choosing

```text
k cards
```

think about leaving behind

```text
n-k consecutive cards.
```

Then

```text
Maximum Score

=

Total Sum

-

Minimum Sum Subarray of Length (n-k)
```

This can be solved using a fixed-size sliding window.

---

# Comparison

| Approach                   | Time     | Space |
| -------------------------- | -------- | ----: |
| Enumerate Left/Right Picks | **O(k)** |  O(1) |
| Total Sum − Minimum Window | O(n)     |  O(1) |

---

# Which Approach is Better?

### Enumeration (your solution)

Advantages:

- Directly models the problem.
- Better when

```text
k << n
```

since it runs in **O(k)**.

---

### Sliding Window

Advantages:

- Elegant mathematical transformation.
- Useful if the "leave a window" perspective comes naturally.

---

# Common Mistake #1

Trying to greedily pick the larger end.

Example:

```text
1 100 3 4
```

Greedy choices do not necessarily produce the optimal answer.

---

# Common Mistake #2

Thinking the picked cards form one contiguous subarray.

They actually consist of

```text
Prefix

+

Suffix
```

---

# Common Mistake #3

Trying recursion or DP.

There are only

```text
k+1
```

possible left/right splits.

Enumeration is sufficient.

---

# Pattern Recognition

Whenever a problem says

```text
Pick elements only from the two ends
```

think:

```text
How many from left?

How many from right?
```

Enumerate all possible splits.

---

# Interview Trigger

```text
Choose exactly K

↓

Only from ends

↓

Left Picks + Right Picks = K

↓

Enumerate all K+1 splits

↓

Maintain left sum & right sum

↓

O(k)
```

---

# Core Insight

Although there are exponentially many **orders** of taking left and right cards, the final result depends only on **how many** cards are taken from each side. Therefore, instead of exploring all pick sequences, enumerate the `k+1` possible `(left picks, right picks)` combinations. Starting with all `k` cards from the left and gradually replacing one left card with one right card visits every valid combination in **O(k)** time.
