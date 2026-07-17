# Next Greater Element II (LeetCode 503)

## References

**Question:**

[https://leetcode.com/problems/next-greater-element-ii/](https://leetcode.com/problems/next-greater-element-ii/)

**Related Problems:**

- Next Greater Element I
- Daily Temperatures
- Stock Span
- Largest Rectangle in Histogram

---

# Pattern

```text
Monotonic Stack

Circular Array

Next Greater Element
```

---

# Problem Statement

Given a **circular array**, find the **Next Greater Element (NGE)** for every element.

If no greater element exists, return

```text
-1
```

---

# Difference from Next Greater Element I

### Next Greater Element I

Search only towards the right.

```text
1 3 2 4
```

For

```text
4
```

Answer

```text
-1
```

---

### Next Greater Element II

Array is circular.

```text
1 3 2 4
        ↑
        |
        └────── wraps to beginning
```

Now every element may continue searching from the beginning.

---

# Key Observation

Instead of physically duplicating the array,

pretend the array is

```text
nums + nums
```

Example

```text
1 2 1

↓

1 2 1 1 2 1
```

But avoid extra space by using

```cpp
index = i % n
```

---

# Core Idea

Traverse

```text
2n-1 → 0
```

instead of

```text
n-1 → 0
```

This simulates one complete wrap around.

---

# Monotonic Stack

Maintain a **monotonically decreasing stack**.

Before processing an element,

remove all smaller (or equal) elements.

```cpp
while(!st.empty() && st.top() <= nums[i])
    st.pop();
```

The remaining top becomes the next greater element.

---

# Why Two Passes?

The traversal naturally divides into two phases.

---

## Phase 1

```text
idx = 2n-1 → n
```

Purpose

```text
Build the stack.
```

No answers are recorded.

The stack stores possible candidates after wrapping around.

---

## Phase 2

```text
idx = n-1 → 0
```

Now the stack already contains information from

- right side
- wrapped-around beginning

So we compute answers.

```cpp
if(idx < n)
    ans[i] = st.empty() ? -1 : st.top();
```

---

# Algorithm

```text
Traverse from 2n-1 to 0

↓

Convert index

i = idx % n

↓

Remove all smaller elements

↓

If second pass

↓

Top = Answer

↓

Push current element
```

---

# Template

```cpp
for(int idx = 2*n-1; idx >= 0; idx--){

    int i = idx % n;

    while(!st.empty() && st.top() <= nums[i])
        st.pop();

    if(idx < n)
        ans[i] = st.empty() ? -1 : st.top();

    st.push(nums[i]);
}
```

---

# Example

```text
nums =

1 2 1
```

Traversal order

```text
idx

5 4 3 | 2 1 0

i

2 1 0 | 2 1 0
```

---

### First Pass

Build stack.

No answers recorded.

---

### Second Pass

Now

```text
1
```

can correctly see

```text
2
```

after wrapping around.

Final answer

```text
2 -1 2
```

---

# Why It Works

The first pass prepares the stack as if the array had already wrapped once.

During the second pass,

every element has access to

- elements to its right
- elements at the beginning

Therefore,

the stack behaves exactly like the normal Next Greater Element problem.

---

# Complexity

```text
Time : O(n)

Space : O(n)
```

Although the loop runs

```text
2n
```

times,

each element is

- pushed at most twice
- popped at most twice

Total operations

```text
O(4n)

= O(n)
```

---

# Common Mistake #1

Traversing only

```text
n-1 → 0
```

This ignores the circular property.

---

# Common Mistake #2

Actually duplicating the array.

Unnecessary.

Simply use

```cpp
i = idx % n;
```

---

# Common Mistake #3

Computing answers during both passes.

Correct template:

```cpp
if(idx < n)
    ans[i] = ...
```

First pass is only for building the stack.

---

# Common Mistake #4

Using `<` instead of `<=`

```cpp
while(st.top() <= nums[i])
```

The problem asks for **strictly greater** element.

Equal elements must also be removed.

---

# Comparison

| Problem                 | Traversal | Circular |
| ----------------------- | --------- | -------- |
| Next Greater Element I  | n-1 → 0   | ❌       |
| Next Greater Element II | 2n-1 → 0  | ✅       |

Everything else remains identical.

---

# Pattern Recognition

Whenever you see

```text
Circular Array

+

Next Greater / Smaller

+

First Greater on Right
```

immediately think

```text
Monotonic Stack

↓

Traverse 2n times

↓

Modulo indexing

↓

Record answers only during second pass
```

---

# Interview Trigger

```text
Circular Array

↓

Need next greater element

↓

Duplicate traversal

↓

i = idx % n

↓

Monotonic Stack

↓

Remove useless elements

↓

Second pass computes answers

↓

O(n)
```

---

# Core Insight

A circular array can be simulated **without duplicating it** by traversing indices from `2n−1` to `0` and accessing elements using `idx % n`. The **first pass** prepares the monotonic stack with candidates from the wrapped-around portion of the array, while the **second pass** computes the actual next greater elements. Apart from this doubled traversal, the algorithm is identical to the standard **Next Greater Element** template.
