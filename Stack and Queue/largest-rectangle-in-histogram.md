# Largest Rectangle in Histogram (LeetCode 84)

## References

**Question:**

[https://leetcode.com/problems/largest-rectangle-in-histogram/](https://leetcode.com/problems/largest-rectangle-in-histogram/)

**Related Problems:**

- Maximal Rectangle
- Sum of Subarray Minimums
- Sum of Subarray Ranges
- Trapping Rain Water
- Daily Temperatures

---

# Pattern

```text id="w6axmg"
Monotonic Increasing Stack

Previous Smaller Element

Next Smaller Element

Boundary Expansion
```

---

# Problem Statement

Given the heights of histogram bars,

find the area of the largest rectangle that can be formed.

---

# Brute Force

Choose every bar as the height.

Expand left until a smaller bar appears.

Expand right until a smaller bar appears.

Compute the area.

---

## Complexity

```text id="b0q7py"
Time  : O(n²)

Space : O(1)
```

---

# Key Observation

Suppose we choose

```text id="jlwmg6"
height[i]
```

as the rectangle height.

The rectangle can extend

- left
- right

until a **smaller** bar blocks it.

Therefore, for every bar we only need

- Previous Smaller Element (PSE)
- Next Smaller Element (NSE)

---

# Approach 1 : Precompute PSE & NSE

For every index,

find

```text id="vh5nqo"
PSE

↓

Nearest smaller bar on the left
```

and

```text id="96ektr"
NSE

↓

Nearest smaller bar on the right
```

Then

```text id="ihg0zl"
Width

=

NSE - PSE - 1
```

Area

```cpp id="gfspbq"
area = height[i] * (nse[i] - pse[i] - 1);
```

Take the maximum.

---

# Algorithm (Approach 1)

```text id="jk5x2g"
Find Previous Smaller

↓

Find Next Smaller

↓

For every bar

↓

Width

=

NSE - PSE - 1

↓

Area

=

Height × Width

↓

Maximum Area
```

---

# Complexity

```text id="rg46u3"
Time  : O(n)

Space : O(n)
```

---

# Can We Improve Space?

Notice something.

When an element is popped from the stack,

we already know

```text id="gxjlwm"
Its Next Smaller Element
```

because the current index is the first smaller bar on the right.

At that exact moment,

after popping,

the stack top automatically becomes

```text id="2gsvtu"
Previous Smaller Element
```

So both boundaries become available immediately.

No arrays are required.

---

# Approach 2 : One Pass Stack

Maintain a **monotonic increasing stack** of indices.

Whenever

```text id="3utcy5"
current height

<

stack top height
```

the bar at the stack top cannot extend any further.

Its rectangle is complete.

Compute its area immediately.

---

# Why Does Pop Give Both Boundaries?

Suppose

```text id="7drig4"
Current Index = i
```

When

```text id="85opcs"
height[i]
```

is smaller,

the popped bar's

```text id="x5x8t2"
Next Smaller

=

i
```

because this is the first smaller element on the right.

After popping,

the new stack top becomes

```text id="prsdxe"
Previous Smaller
```

since everything larger has already been removed.

Thus

```text id="nysuf5"
Current Index
        ↓
      NSE

Popped Bar

New Stack Top
        ↓
      PSE
```

Both boundaries are obtained in one pop.

---

# Area Formula

For the popped index

```text id="38z61e"
ind
```

```cpp id="l78gv9"
nse = currentIndex;

pse = stack.empty() ? -1 : stack.top();

width = nse - pse - 1;

area = heights[ind] * width;
```

---

# Remaining Stack

Some bars never encounter a smaller element.

For them,

```text id="9edkjr"
NSE = n
```

Process the remaining stack after traversal.

---

# Algorithm (Approach 2)

```text id="g7lzjlwm"
Traverse histogram

↓

Maintain increasing stack

↓

Current bar smaller?

↓

Pop

↓

Current index = NSE

↓

Stack top = PSE

↓

Compute area

↓

Push current index

↓

After traversal

↓

Pop remaining bars

↓

NSE = n

↓

Maximum Area
```

---

# Example

```text id="bq9vlk"
heights

2 1 5 6 2 3
```

When

```text id="k2h0gh"
2
```

arrives,

bars

```text id="cljlwm"
6

5
```

are popped.

For

```text id="y25m9r"
6
```

```text id="r1twk7"
NSE = current index

PSE = index of 5
```

Area is computed immediately.

Similarly,

for

```text id="1tjk1m"
5
```

its boundaries are now known.

---

# Why It Works

A bar remains in the stack as long as no smaller bar appears.

The moment a smaller bar is found,

that bar can never extend further to the right.

Its largest possible rectangle is now fixed,

so its area can be computed immediately.

---

# Complexity

| Approach         | Time | Space |
| ---------------- | ---- | ----: |
| PSE + NSE Arrays | O(n) |  O(n) |
| One Pass Stack   | O(n) |  O(n) |

Although the second approach removes the boundary arrays, the stack still requires **O(n)** space.

---

# Common Mistake #1

Comparing indices instead of heights.

Wrong

```cpp id="5vgns6"
while(!st.empty() && st.top() >= heights[i])
```

Correct

```cpp id="ukgm34"
while(!st.empty() && heights[st.top()] >= heights[i])
```

The stack stores **indices**, not heights.

---

# Common Mistake #2

Using

```cpp id="vutij9"
if(...)
```

instead of

```cpp id="zwjlwm"
while(...)
```

A single incoming bar may terminate multiple rectangles.

---

# Common Mistake #3

Forgetting to process the remaining stack.

Bars that never find a smaller element have

```text id="zkz9o0"
NSE = n
```

---

# Common Mistake #4

Using the current index as the width.

Correct width is

```text id="2kcxxq"
NSE - PSE - 1
```

not

```text id="hzg9ha"
NSE - PSE
```

---

# Comparison

| Problem                        | Contribution Based | Boundary Based |
| ------------------------------ | -----------------: | -------------: |
| Sum of Subarray Minimums       |                 ✅ |             ❌ |
| Sum of Subarray Ranges         |                 ✅ |             ❌ |
| Largest Rectangle in Histogram |                 ❌ |             ✅ |

---

# Pattern Recognition

Whenever you see

```text id="4isjlwm"
Need nearest smaller element

↓

Need maximum expansion

↓

Rectangle / Width / Span
```

think

```text id="jlwm6u"
Monotonic Increasing Stack
```

If the problem asks for the **largest span** satisfying a condition, a monotonic stack is often the right tool.

---

# Interview Trigger

```text id="jlwm8w"
Each bar is the rectangle height

↓

Need left boundary

↓

Need right boundary

↓

Width = NSE - PSE - 1

↓

Area = Height × Width

↓

Approach 1:
PSE + NSE arrays

↓

Approach 2:
One monotonic stack

↓

Pop ⇒ Current index is NSE

↓

New stack top is PSE

↓

Compute area immediately

↓

O(n)
```

---

# Core Insight

Every rectangle is uniquely determined by choosing one bar as its **minimum height**. The rectangle extends left and right until a smaller bar blocks it. While this can be solved by explicitly computing Previous Smaller and Next Smaller arrays, the one-pass monotonic stack is more elegant: **when a bar is popped, the current index instantly becomes its Next Smaller Element, and the new stack top becomes its Previous Smaller Element**. This allows the maximum area to be computed immediately in a single traversal while maintaining **O(n)** time complexity.
