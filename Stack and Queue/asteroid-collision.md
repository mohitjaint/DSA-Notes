# Asteroid Collision (LeetCode 735)

## References

**Question:**

[https://leetcode.com/problems/asteroid-collision/](https://leetcode.com/problems/asteroid-collision/)

**Related Problems:**

- Next Greater Element
- Daily Temperatures
- Remove K Digits
- Robot Collisions
- Basic Calculator

---

# Pattern

```text
Stack

Simulation

Collision Processing
```

---

# Problem Statement

Asteroids move in a straight line.

- Positive → moving right
- Negative → moving left

When a right-moving asteroid meets a left-moving asteroid:

- Smaller one explodes.
- If equal, both explode.
- Asteroids moving in the same direction never collide.

Return the final state after all collisions.

---

# Brute Force

Repeatedly scan the array.

Whenever two adjacent asteroids collide,

resolve the collision and start over.

---

## Complexity

```text
Time  : O(n²)

Space : O(1)
```

---

# Key Observation

An asteroid can collide **only with the nearest surviving asteroid moving towards it**.

That means:

- A positive asteroid only needs to worry about future negative asteroids.
- A negative asteroid only needs to check previously seen positive asteroids.

This "most recent unresolved object" behavior naturally suggests a **stack**.

---

# Core Idea

Maintain a stack of asteroids that have survived so far.

When processing a new asteroid:

### Positive asteroid

```cpp
st.push_back(x);
```

No collision is possible yet.

---

### Negative asteroid

It may collide with positive asteroids already in the stack.

Resolve collisions until one of the following happens.

---

# Case 1 : Destroy Smaller Positive Asteroids

While

```text
Top > 0

AND

Top < |current|
```

the positive asteroid explodes.

```cpp
while(!st.empty() &&
      st.back() > 0 &&
      st.back() < abs(x))
{
    st.pop_back();
}
```

---

# Case 2 : Equal Size

If

```text
Top == |current|
```

both explode.

```cpp
st.pop_back();
```

Do **not** push the current asteroid.

---

# Case 3 : Current Asteroid Survives

If

```text
Stack is empty

OR

Top is negative
```

there is no possible collision.

Push it.

```cpp
st.push_back(x);
```

---

# Case 4 : Current Asteroid Dies

If

```text
Top > |current|
```

the positive asteroid survives.

The current asteroid disappears.

No action is required.

---

# Algorithm

```text
For every asteroid

↓

Positive ?

↓

Push into stack

--------------------

Negative ?

↓

Destroy smaller positive asteroids

↓

Equal size ?

↓

Destroy both

↓

Else

Stack empty OR top negative ?

↓

Push current asteroid

↓

Otherwise

Current asteroid dies
```

---

# Example

```text
Input

[5, 10, -5]
```

Process

```text
5

↓

[5]

10

↓

[5,10]

-5

↓

10 survives

↓

[5,10]
```

Answer

```text
[5,10]
```

---

Another example

```text
Input

[10,2,-5]
```

Process

```text
10

↓

[10]

2

↓

[10,2]

-5

↓

2 destroyed

↓

10 survives

↓

[10]
```

---

Another example

```text
Input

[8,-8]
```

Process

```text
8

↓

[8]

-8

↓

Both destroyed

↓

[]
```

---

# Why It Works

The stack always contains the asteroids that are still alive.

A collision is only possible when

```text
Top > 0

Current < 0
```

because they are moving toward each other.

Every collision is resolved immediately before processing the next asteroid, so the stack always represents the correct surviving state.

---

# Complexity

```text
Time  : O(n)

Space : O(n)
```

Although there is a `while` loop, every asteroid is:

- pushed at most once,
- popped at most once.

Total stack operations are at most `2n`.

---

# Common Mistake #1

Checking collisions for every pair.

Only

```text
Positive on stack

+

Current negative
```

can collide.

---

# Common Mistake #2

Stopping after one collision.

Example

```text
[5,3,-8]
```

`-8` destroys both `3` and `5`.

Hence a `while` loop is required.

---

# Common Mistake #3

Pushing the negative asteroid immediately after destroying one asteroid.

Example

```text
[10,2,-5]
```

After destroying `2`, `-5` must still collide with `10`.

---

# Common Mistake #4

Forgetting the equal-size case.

```text
8

-8
```

Both should disappear.

---

# Common Mistake #5

Using a queue instead of a stack.

Only the **most recently survived positive asteroid** can collide first with the incoming negative asteroid.

A stack models this perfectly.

---

# Why a Stack?

Suppose

```text
5 10 -5
```

The asteroid `-5` cannot collide with `5` before colliding with `10`.

The nearest surviving asteroid is always processed first.

This **Last-In, First-Out (LIFO)** behavior is exactly what a stack provides.

---

# Comparison

| Problem              | Pattern               | Data Structure |
| -------------------- | --------------------- | -------------- |
| Next Greater Element | Monotonic Stack       | Stack          |
| Daily Temperatures   | Monotonic Stack       | Stack          |
| Asteroid Collision   | Simulation            | Stack          |
| Remove K Digits      | Greedy + Stack        | Stack          |
| Basic Calculator     | Expression Evaluation | Stack          |

---

# Pattern Recognition

Whenever you see:

```text
Objects arrive one by one

↓

Only the most recent unresolved object can interact

↓

Interactions may repeat until stable
```

think:

```text
Stack

+

Simulation
```

---

# Interview Trigger

```text
Asteroids move left/right

↓

Only (+ , -) can collide

↓

Keep surviving asteroids in a stack

↓

Incoming negative destroys smaller positives

↓

Equal size → both destroyed

↓

Stack empty or top negative → push

↓

Otherwise current asteroid dies

↓

O(n)
```

---

# Core Insight

Instead of simulating collisions between every pair of asteroids, maintain only the **surviving asteroids** in a stack. When a left-moving asteroid arrives, it can only collide with the **nearest surviving right-moving asteroid** (the stack top). Resolve collisions until the current asteroid is destroyed or safely survives. Since every asteroid is pushed and popped at most once, the overall time complexity is **O(n)**.
