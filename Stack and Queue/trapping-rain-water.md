# Trapping Rain Water (LeetCode 42)

## References

**Question:**

[https://leetcode.com/problems/trapping-rain-water/](https://leetcode.com/problems/trapping-rain-water/)

**Related Problems:**

- Container With Most Water
- Maximum Width Ramp
- Trapping Rain Water II (Priority Queue)
- Largest Rectangle in Histogram

---

# Pattern

```text
Two Pointers

Prefix Maximum

Space Optimization
```

---

# Problem Statement

Given an elevation map,

compute the total amount of rainwater trapped after raining.

---

# Brute Force

For every index

find

- maximum bar on the left
- maximum bar on the right

Water trapped

```text
min(leftMax, rightMax) - height[i]
```

---

## Complexity

```text
Time  : O(n²)

Space : O(1)
```

---

# Better Solution

Precompute

```text
leftMax[]
rightMax[]
```

Then

```text
water[i] = min(leftMax[i], rightMax[i]) - height[i]
```

---

## Complexity

```text
Time  : O(n)

Space : O(n)
```

---

# Key Observation

At every position,

the trapped water depends on

```text
min(leftMax, rightMax)
```

The question is:

> Can we avoid storing both arrays?

Yes.

---

# Core Insight

Maintain

```cpp
leftMax
rightMax
```

and two pointers

```cpp
l
r
```

---

Suppose

```text
height[l] < height[r]
```

Then there is already a bar on the right that is **at least as tall as `height[r]`**, and therefore taller than `height[l]`.

The amount of water at index `l` depends only on

```text
leftMax
```

because:

- if `leftMax` is smaller, it limits the water level.
- if `rightMax` is smaller, it is still at least `height[r]`, which is already higher than `height[l]`.

Therefore,

we can safely compute water at `l` immediately.

Move

```cpp
l++
```

---

Similarly,

if

```text
height[r] <= height[l]
```

the water at

```text
r
```

depends only on

```text
rightMax
```

Move

```cpp
r--
```

---

# Algorithm

```text
Initialize

leftMax = 0
rightMax = 0

l = 0
r = n-1

↓

If height[l] < height[r]

↓

Update leftMax

↓

Add trapped water

↓

Move l

-------------------

Else

↓

Update rightMax

↓

Add trapped water

↓

Move r
```

---

# Formula

If

```text
height[l] < leftMax
```

then

```cpp
water += leftMax - height[l];
```

Otherwise

```cpp
leftMax = height[l];
```

Similarly,

```cpp
water += rightMax - height[r];
```

or update

```cpp
rightMax
```

---

# Example

```text
height

4 2 0 3 2 5
```

Initially

```text
leftMax = 0

rightMax = 0
```

Process left side

```text
4

leftMax = 4
```

Next

```text
2

water += 4-2 = 2
```

Next

```text
0

water += 4
```

Continue similarly.

Final answer

```text
9
```

---

# Why It Works

The water level at any position is

```text
min(leftMax, rightMax)
```

When

```text
height[l] < height[r]
```

the left side is guaranteed to be the limiting side.

Therefore,

the exact value of

```text
rightMax
```

does not matter.

Only

```text
leftMax
```

is needed.

Symmetric reasoning applies to the right side.

---

# Complexity

```text
Time  : O(n)

Space : O(1)
```

---

# Common Mistake #1

Writing

```cpp
total = leftMax - height[l];
```

instead of

```cpp
total += leftMax - height[l];
```

Water accumulates across multiple positions.

---

# Common Mistake #2

Comparing

```text
leftMax
```

and

```text
rightMax
```

instead of

```text
height[l]
```

and

```text
height[r]
```

The two-pointer proof relies on the current boundary heights.

---

# Common Mistake #3

Updating

```text
leftMax
```

after adding water.

Correct order:

```text
Update maximum

↓

Compute trapped water
```

(or equivalently, if current height is below the maximum, add water; otherwise update the maximum).

---

# Comparison

| Approach          | Time  | Space |
| ----------------- | ----- | ----: |
| Brute Force       | O(n²) |  O(1) |
| Prefix Max Arrays | O(n)  |  O(n) |
| Two Pointers      | O(n)  |  O(1) |

---

# Pattern Recognition

Whenever you see

```text
Need left information

+

Need right information

+

Formula uses

min(left, right)
```

ask:

> Can one side be determined without knowing the other?

If yes,

a **two-pointer space optimization** may exist.

---

# Interview Trigger

```text
Rain Water

↓

Need left maximum

Need right maximum

↓

Prefix arrays?

↓

Can optimize space?

↓

Two pointers

↓

Maintain leftMax & rightMax

↓

Move the side with smaller height

↓

O(n) time

O(1) space
```

---

# Core Insight

Although the trapped water at an index is `min(leftMax, rightMax) - height[i]`, we **do not need both maxima simultaneously**. By always processing the side with the **smaller current height**, we know that side's maximum alone is sufficient to determine the water trapped there. This observation eliminates the need for prefix/suffix arrays, reducing the space complexity from **O(n)** to **O(1)** while keeping the time complexity **O(n)**.
