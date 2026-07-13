# Min Stack (LeetCode 155)

## References

**Question:**

[https://leetcode.com/problems/min-stack/](https://leetcode.com/problems/min-stack/)

**Related Problems:**

- Design a Stack With Increment Operation
- Max Stack
- Implement Queue using Stacks
- Implement Stack using Queues

---

# Pattern

```text
Stack

Design Data Structure

Mathematical Encoding
```

---

# Problem Statement

Design a stack supporting the following operations in **O(1)** time:

- Push
- Pop
- Top
- Get Minimum

---

# Brute Force

Maintain a stack.

Whenever

```text
getMin()
```

is called,

scan the entire stack.

---

## Complexity

```text
Push : O(1)

Pop : O(1)

Top : O(1)

getMin : O(n)
```

Too slow.

---

# Better Solution

Maintain

```text
Main Stack

+

Min Stack
```

Every push also updates the minimum stack.

---

## Complexity

```text
Time : O(1)

Space : O(n)
```

---

# Optimal Solution

Use **only one stack**.

Instead of storing every value directly,

store an **encoded value** whenever a new minimum appears.

---

# Core Idea

Maintain

```cpp
mini
```

which stores the current minimum.

---

## Case 1

New value is **not** a new minimum.

```text
val >= mini
```

Store normally.

```cpp
st.push(val);
```

---

## Case 2

New value becomes the new minimum.

```text
val < mini
```

Instead of storing

```text
val
```

store

```text
encoded = 2 × val − mini
```

Then

```cpp
mini = val;
```

---

# Why Encoding Works

Suppose

```text
old minimum = oldMin

new minimum = val
```

Store

```text
encoded = 2×val − oldMin
```

Since

```text
val < oldMin
```

we get

```text
encoded < val
```

Therefore

```text
encoded < current minimum
```

This acts as a **marker** indicating that the top of the stack is encoded.

---

# Push Algorithm

```text
If stack empty

↓

mini = val

↓

push(val)

-----------------

Else

↓

If val ≥ mini

↓

push(val)

-----------------

Else

↓

push(2×val−mini)

↓

mini = val
```

---

# Pop Algorithm

Read

```cpp
x = st.top()
```

---

## Normal Value

```text
x ≥ mini
```

Simply pop.

---

## Encoded Value

```text
x < mini
```

Recover previous minimum.

Formula

```text
oldMin = 2×currentMin − encoded
```

Implementation

```cpp
mini = 2LL * mini - x;
```

Then pop.

---

# Top Algorithm

If

```text
x ≥ mini
```

return

```text
x
```

Else

return

```text
mini
```

because the encoded value actually represents the current minimum.

---

# Why Decoding Works

Encoding:

```text
encoded = 2×newMin − oldMin
```

Rearranging,

```text
oldMin = 2×newMin − encoded
```

Exactly the formula used while popping.

---

# Example

Operations

```text
push(5)

push(3)

push(7)

push(2)
```

---

After push(5)

```text
Stack

5

mini = 5
```

---

After push(3)

Store

```text
2×3−5 = 1
```

```text
Stack

1
5

mini = 3
```

Notice

```text
1 < mini
```

Therefore

```text
1
```

is an encoded value.

---

After push(7)

```text
Stack

7
1
5

mini = 3
```

---

After push(2)

Store

```text
2×2−3 = 1
```

```text
Stack

1
7
1
5

mini = 2
```

---

Pop

Top

```text
1
```

Since

```text
1 < mini
```

decode

```text
oldMin = 2×2−1 = 3
```

Restore

```text
mini = 3
```

---

# Complexity

| Operation | Time | Space |
| --------- | ---- | ----: |
| Push      | O(1) |  O(1) |
| Pop       | O(1) |  O(1) |
| Top       | O(1) |  O(1) |
| GetMin    | O(1) |  O(1) |

---

# Important Implementation Detail

The encoded value

```text
2×val−mini
```

may exceed the range of

```cpp
int
```

Example

```text
val = INT_MIN

mini = INT_MAX
```

Encoded value

```text
2×INT_MIN−INT_MAX

≈ -6442450943
```

which **does not fit inside an `int`**.

---

# Correct Data Types

Use

```cpp
stack<long long> st;
long long mini;
```

Also force arithmetic into

```cpp
long long
```

using

```cpp
2LL * val
```

instead of

```cpp
2 * val
```

---

# Common Mistake #1

Using

```cpp
stack<int>
```

The encoded value may overflow and become corrupted.

---

# Common Mistake #2

Writing

```cpp
2 * val
```

Both operands are `int`, so multiplication happens in 32-bit arithmetic and can overflow **before** being assigned to a `long long`.

Correct:

```cpp
2LL * val
```

---

# Common Mistake #3

Making only

```cpp
mini
```

a `long long`.

Both **storage** and **computation** must use 64-bit integers.

---

# Comparison

| Approach        | Time | Extra Space |
| --------------- | ---- | ----------: |
| Scan every time | O(n) |        O(1) |
| Two stacks      | O(1) |        O(n) |
| Encoding trick  | O(1) |        O(1) |

---

# Pattern Recognition

Whenever a design problem asks for

```text
O(1)

minimum / maximum retrieval
```

consider:

1. Auxiliary stack
2. Mathematical encoding

If an encoding formula is used,

always check:

```text
Can the encoded value exceed the original data type?
```

If yes,

widen **both**

- computation
- storage

---

# Interview Trigger

```text
Design Stack

↓

O(1) getMin()

↓

Two stacks?

↓

Can we reduce space?

↓

Encode previous minimum

↓

Store transformed values

↓

Decode during pop

↓

O(1) time

O(1) extra space
```

---

# Core Insight

Instead of storing the new minimum directly, store a specially **encoded value** that implicitly remembers the previous minimum. The key property is that every encoded value is **strictly smaller than the current minimum**, allowing it to be identified during `pop()` and `top()`. This enables all operations to run in **O(1)** time while using only a **single stack** and constant extra space.
