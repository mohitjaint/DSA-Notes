# Sliding Window Maximum (LeetCode 239)

## References

**Question:**

[https://leetcode.com/problems/sliding-window-maximum/](https://leetcode.com/problems/sliding-window-maximum/)

**Related Problems:**

- Sliding Window Minimum
- First Negative Number in Every Window
- Longest Continuous Subarray With Absolute Diff ≤ Limit
- Daily Temperatures
- Next Greater Element

---

# Pattern

```text
Monotonic Deque

Sliding Window

Maximum in Every Window
```

---

# Problem Statement

Given an array and a window of size

```text
k
```

find the maximum element in every sliding window.

---

# Brute Force

For every window,

scan all

```text
k
```

elements.

Find the maximum.

---

## Complexity

```text
Time  : O(nk)

Space : O(1)
```

---

# Better Solution

Maintain a balanced BST / Multiset.

Insert new element.

Remove outgoing element.

Largest element is always available.

---

## Complexity

```text
Time  : O(n log k)

Space : O(k)
```

---

# Key Observation

Suppose the current window is

```text
3 1 2 5
```

When

```text
5
```

arrives,

both

```text
1

2
```

can never become the maximum of any future window.

Why?

Because

- they are smaller
- they are older

So they are completely useless.

Discard them immediately.

---

# Core Idea

Maintain a deque containing **indices**.

The deque satisfies two properties.

---

## Property 1

All indices belong to the current window.

Before processing the current index

```cpp
if(!dq.empty() && dq.front() <= i-k)
    dq.pop_front();
```

Remove expired indices.

---

## Property 2

Values are stored in **decreasing order**.

Whenever a new element arrives,

remove all smaller elements from the back.

```cpp
while(!dq.empty() && nums[dq.back()] <= nums[i])
    dq.pop_back();
```

Then insert

```cpp
dq.push_back(i);
```

---

# Why Remove Smaller Elements?

Suppose

```text
Deque

9 7 5

Current

8
```

The

```text
5
```

will never become the maximum because

- it is smaller than

```text
8
```

- it is older than

```text
8
```

So remove it.

Result

```text
9 7

↓

9

↓

9 8
```

The deque remains decreasing.

---

# Why Store Indices Instead of Values?

Indices allow us to determine whether an element has left the window.

Example

```text
Window size = 3

Index

0 1 2 3
```

When processing

```text
3
```

index

```text
0
```

must be removed.

Without indices,

we cannot know whether the value belongs to the current window.

---

# Why Is the Front Always the Maximum?

The deque always satisfies

```text
Front

↓

Largest value

↓

Smaller values

↓

Smallest value
```

The front never has a larger value behind it.

Expired indices are removed immediately.

Therefore

```cpp
nums[dq.front()]
```

is always the maximum of the current window.

---

# Algorithm

```text
Traverse array

↓

Remove expired indices

↓

Remove all smaller elements from the back

↓

Push current index

↓

Window formed?

↓

Answer = nums[dq.front()]
```

---

# Example

```text
nums

1 3 -1 -3 5 3 6 7

k = 3
```

---

Window

```text
1 3 -1
```

Deque

```text
3 -1
```

Maximum

```text
3
```

---

Next window

```text
3 -1 -3
```

Deque

```text
3 -1 -3
```

Maximum

```text
3
```

---

Next window

```text
-1 -3 5
```

When

```text
5
```

arrives,

remove

```text
-3

-1
```

Deque

```text
5
```

Maximum

```text
5
```

Continue similarly.

---

# Why It Works

Every element enters the deque exactly once.

It is removed only when

- it becomes smaller than a newer element, or
- it leaves the current window.

Therefore,

each element is processed at most twice.

---

# Complexity

```text
Time  : O(n)

Space : O(k)
```

Although there is a nested `while` loop,

each index is

- pushed once,
- popped from the back at most once,
- popped from the front at most once.

Total operations remain linear.

---

# Common Mistake #1

Storing values instead of indices.

Wrong

```cpp
deque<int> dq;
dq.push_back(nums[i]);
```

Correct

```cpp
deque<int> dq;
dq.push_back(i);
```

---

# Common Mistake #2

Removing expired indices after computing the answer.

Always remove expired indices **before** using the front.

---

# Common Mistake #3

Using `<` instead of `<=`

Correct

```cpp
while(!dq.empty() && nums[dq.back()] <= nums[i])
```

This removes duplicate values and keeps the newer occurrence, which stays in the window longer.

---

# Common Mistake #4

Using an `if` instead of a `while`

Wrong

```cpp
if(nums[dq.back()] <= nums[i])
```

Multiple smaller elements may need to be removed.

---

# Comparison

| Approach        | Time       | Space |
| --------------- | ---------- | ----: |
| Brute Force     | O(nk)      |  O(1) |
| Set / Multiset  | O(n log k) |  O(k) |
| Monotonic Deque | O(n)       |  O(k) |

---

# Monotonic Stack vs Monotonic Deque

| Monotonic Stack                  | Monotonic Deque                   |
| -------------------------------- | --------------------------------- |
| One active end                   | Two active ends                   |
| Used for nearest greater/smaller | Used for sliding windows          |
| Pop from top when order breaks   | Pop from back when order breaks   |
| No concept of expiry             | Remove expired indices from front |
| Answer often produced on pop     | Answer always at the front        |

---

# Pattern Recognition

Whenever you see

```text
Sliding window

+

Need maximum/minimum

+

Elements continuously enter and leave
```

think

```text
Monotonic Deque
```

The deque maintains

- **monotonic order** from the back,
- **window validity** from the front.

---

# Interview Trigger

```text
Sliding Window Maximum

↓

Need maximum in every window

↓

Maintain decreasing deque

↓

Store indices

↓

Front removes expired indices

↓

Back removes smaller elements

↓

Front always stores maximum

↓

O(n)
```

---

# Core Insight

A smaller element behind a larger, newer element can **never** become the maximum of any future window—it is both **dominated in value** and **older**, so it will leave the window earlier. Therefore, such elements are removed immediately from the back of the deque. The front of the deque always holds the maximum element of the current valid window, giving an **O(n)** solution where every index is inserted and removed at most once.
