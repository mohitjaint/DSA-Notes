# Next Greater Element I (LeetCode 496)

## References

**Question:**

[https://leetcode.com/problems/next-greater-element-i/](https://leetcode.com/problems/next-greater-element-i/)

**Related Problems:**

- Next Greater Element II
- Daily Temperatures
- Stock Span Problem
- Largest Rectangle in Histogram

---

# Pattern

```text
Monotonic Stack

Next Greater Element

Right-to-Left Traversal
```

---

# Problem Statement

For every element in

```text
nums1
```

find its **Next Greater Element (NGE)** in

```text
nums2
```

The NGE is the **first greater element to its right**.

If none exists,

```text
Answer = -1
```

---

# Brute Force

For every element,

search towards the right until a greater element is found.

---

## Complexity

```text
Time : O(n²)

Space : O(1)
```

---

# Key Observation

While processing from right to left,

many elements become useless.

Example

```text
5 3 4
```

When processing

```text
5
```

both

```text
3
```

and

```text
4
```

can never be its answer.

Similarly,

for future elements,

```text
3
```

can never be useful because

```text
4
```

is closer and larger.

So remove useless elements.

---

# Core Idea

Maintain a stack such that

```text
Stack is monotonically decreasing
(from bottom to top)
```

Example

```text
Bottom

8
6
4
2

Top
```

---

# Why Monotonic?

Suppose stack is

```text
8
5
2
```

Current element

```text
6
```

Can

```text
5
```

ever become the next greater element for anything to the left?

No.

Because

```text
6
```

is

- closer
- larger

Therefore,

```text
5
```

becomes useless forever.

Pop it.

---

# Algorithm

Traverse from

```text
Right → Left
```

For every element

---

## Step 1

Remove all smaller (or equal) elements.

```cpp
while(!st.empty() && st.top() <= nums[i])
    st.pop();
```

---

## Step 2

If stack becomes empty

```text
No greater element exists.
```

Otherwise

```text
Top of stack = Next Greater Element
```

---

## Step 3

Push current element.

```cpp
st.push(nums[i]);
```

---

# Complete Flow

```text
Traverse from right

↓

Remove useless elements

↓

Top becomes answer

↓

Push current element

↓

Continue
```

---

# Example

```text
nums =

2 1 3 5 4
```

---

Process

```text
4
```

Stack

```text
4
```

Answer

```text
-1
```

---

Process

```text
5
```

Remove

```text
4
```

Stack empty

Answer

```text
-1
```

Push

```text
5
```

---

Process

```text
3
```

Top

```text
5
```

Answer

```text
5
```

Push

```text
3
```

Stack

```text
5
3
```

---

Process

```text
1
```

Top

```text
3
```

Answer

```text
3
```

Push

```text
1
```

Stack

```text
5
3
1
```

---

Process

```text
2
```

Remove

```text
1
```

Top

```text
3
```

Answer

```text
3
```

Push

```text
2
```

Final Stack

```text
5
3
2
```

---

# Why It Works

The stack always contains only those elements that can potentially become the next greater element for future numbers.

Any element that is

- smaller
- and farther away

than the current element

can never become an answer again.

Hence it is removed permanently.

---

# Complexity

```text
Time : O(n)

Space : O(n)
```

---

# Why O(n)?

Although there is a

```cpp
while(...)
```

inside the loop,

each element

- is pushed once
- is popped once

Therefore,

total operations are

```text
2n
```

which is

```text
O(n)
```

---

# Common Mistake #1

Traversing from left.

Future information is unavailable.

The stack technique naturally works from

```text
Right → Left
```

---

# Common Mistake #2

Popping only smaller elements.

Use

```cpp
<=
```

instead of

```cpp
<
```

because the problem asks for the **next greater** element, not greater-or-equal.

```cpp
while(st.top() <= nums[i])
```

---

# Common Mistake #3

Thinking the stack stores answers.

It stores only

```text
Candidate Next Greater Elements
```

The answer is simply

```text
Current Stack Top
```

after removing useless elements.

---

# Pattern Recognition

Whenever you see

```text
Next Greater

Next Smaller

Previous Greater

Previous Smaller
```

immediately think

```text
Monotonic Stack
```

The only things that change are

- traversal direction
- increasing/decreasing stack
- greater/smaller comparison

---

# Comparison

| Problem                  | Traversal    | Stack Type           |
| ------------------------ | ------------ | -------------------- |
| Next Greater Element     | Right → Left | Monotonic Decreasing |
| Next Smaller Element     | Right → Left | Monotonic Increasing |
| Previous Greater Element | Left → Right | Monotonic Decreasing |
| Previous Smaller Element | Left → Right | Monotonic Increasing |

---

# Template

```cpp
for(int i = n-1; i >= 0; i--){

    while(!st.empty() && st.top() <= nums[i])
        st.pop();

    if(st.empty())
        ans[i] = -1;
    else
        ans[i] = st.top();

    st.push(nums[i]);
}
```

---

# Interview Trigger

```text
Next Greater / Smaller

↓

First greater/smaller on one side

↓

Monotonic Stack

↓

Traverse from the opposite side

↓

Remove useless elements

↓

Stack top = Answer

↓

Push current element

↓

O(n)
```

---

# Core Insight

The stack is **not storing all previous elements**—it stores only the **useful candidates** that can still serve as the next greater element for future positions. Before processing each element, all smaller (or equal) elements are discarded because the current element is both **closer** and **at least as large**, making those elements permanently irrelevant. This "discard useless candidates" principle is the foundation of almost every monotonic stack problem.
