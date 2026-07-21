# Sum of Subarray Minimums (LeetCode 907)

## References

**Question:**

[https://leetcode.com/problems/sum-of-subarray-minimums/](https://leetcode.com/problems/sum-of-subarray-minimums/)

**Related Problems:**

- Sum of Subarray Ranges
- Largest Rectangle in Histogram
- Maximum Score of a Good Subarray
- Sum of Subarray Maximums

---

# Pattern

```text
Monotonic Stack

Contribution Technique

Previous / Next Smaller Element
```

---

# Problem Statement

For every subarray,

find its minimum element.

Return the **sum of all subarray minimums**.

---

# Brute Force

Generate every subarray.

Find its minimum.

Add to answer.

---

## Complexity

```text
Time  : O(n²)

Space : O(1)
```

---

# Key Observation

Instead of asking

```text
For every subarray,
what is its minimum?
```

reverse the thinking.

Ask

```text
For every element,

In how many subarrays
is this element the minimum?
```

Then

```text
Contribution

=

value × number of subarrays
```

---

# Core Idea

Suppose

```text
arr[i]
```

is the minimum.

It can expand

- to the left
- to the right

until a smaller element blocks it.

---

Example

```text
3 1 2 4
```

For

```text
1
```

Left choices

```text
3
1
```

Right choices

```text
2
4
```

Every combination forms a valid subarray.

---

# Contribution Formula

Suppose

```text
left

=

number of choices on left
```

and

```text
right

=

number of choices on right
```

Then

```text
Contribution

=

left × right × arr[i]
```

---

# Computing Left Choices

Find

```text
PSEE

Previous Smaller or Equal Element
```

Then

```text
left = i - psee[i]
```

---

# Computing Right Choices

Find

```text
NSE

Next Smaller Element
```

Then

```text
right = nse[i] - i
```

---

# Final Formula

```cpp
left = i - psee[i];

right = nse[i] - i;

answer += left * right * arr[i];
```

---

# Why Duplicates Need Special Care

Consider

```text
2 2
```

Both

```text
2
```

cannot claim

```text
[2,2]
```

Otherwise,

that subarray is counted twice.

One side must "own" duplicate values.

---

# Valid Tie-Breaking

There are two correct combinations.

### Option 1 (Used in your solution)

| Previous         | Next    |
| ---------------- | ------- |
| Smaller or Equal | Smaller |

Implementation

```cpp
PSEE

pop while >

------------------

NSE

pop while >=
```

---

### Option 2

| Previous | Next             |
| -------- | ---------------- |
| Smaller  | Smaller or Equal |

Implementation

```cpp
PSE

pop while >=

------------------

NSEE

pop while >
```

Either one is correct.

---

# Why?

Suppose

```text
2 2
```

Using

```text
>

>=
```

Only one of the duplicate elements claims

```text
[2,2]
```

No overcounting.

No undercounting.

---

# Algorithm

```text
Find Previous Smaller or Equal

↓

Find Next Smaller

↓

For every index

↓

left = distance to PSEE

↓

right = distance to NSE

↓

Contribution

=

left × right × value

↓

Add to answer
```

---

# Example

```text
arr

3 1 2 4
```

For

```text
1
```

Previous Smaller or Equal

```text
-1
```

Next Smaller

```text
4
```

Left

```text
2
```

Right

```text
3
```

Contribution

```text
2 × 3 × 1 = 6
```

Meaning

```text
1
```

is the minimum in

```text
6
```

different subarrays.

---

# Complexity

```text
Time  : O(n)

Space : O(n)
```

Each index is

- pushed once
- popped once

from each stack.

---

# Common Mistake #1

Using the same inequality on both sides.

Wrong

```cpp
>

>
```

or

```cpp
>=

>=
```

Duplicates are counted incorrectly.

---

# Common Mistake #2

Confusing Previous and Next traversal.

```text
Previous

↓

Left → Right

------------------

Next

↓

Right → Left
```

---

# Common Mistake #3

Forgetting modulo while accumulating.

Wrong

```cpp
ans += contribution;
```

Correct

```cpp
ans = (ans + contribution) % MOD;
```

---

# Common Mistake #4

Using `int` for multiplication.

```cpp
left * right * arr[i]
```

can overflow.

Always use

```cpp
long long
```

for intermediate computations.

---

# Comparison

| Problem                  | Contribution Based? | Stack |
| ------------------------ | ------------------: | ----: |
| Next Greater Element     |                  ❌ |    ✅ |
| Daily Temperatures       |                  ❌ |    ✅ |
| Largest Rectangle        |                  ✅ |    ✅ |
| Sum of Subarray Minimums |                  ✅ |    ✅ |
| Sum of Subarray Ranges   |                  ✅ |    ✅ |

---

# Pattern Recognition

Whenever you see

```text
Sum of

Minimum / Maximum

over all subarrays
```

think

```text
Don't enumerate subarrays.

↓

For every element,

count how many subarrays
it contributes to.

↓

Contribution Technique

+

Monotonic Stack
```

---

# Interview Trigger

```text
Sum over all subarrays

↓

Reverse thinking

↓

For each element

↓

Count valid left choices

↓

Count valid right choices

↓

Contribution

=

left × right × value

↓

Previous / Next Smaller

↓

Monotonic Stack

↓

O(n)
```

---

# Core Insight

Instead of computing the minimum for every subarray, compute **how many subarrays each element serves as the minimum**. The number of such subarrays equals the product of the number of valid choices on the left and the right. Monotonic stacks efficiently find these boundaries in linear time. Handling duplicates correctly is crucial—**one side must use a strict comparison and the other a non-strict comparison** to ensure every subarray is counted exactly once.
