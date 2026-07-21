# Sum of Subarray Ranges (LeetCode 2104)

## References

**Question:**

[https://leetcode.com/problems/sum-of-subarray-ranges/](https://leetcode.com/problems/sum-of-subarray-ranges/)

**Related Problems:**

- Sum of Subarray Minimums
- Largest Rectangle in Histogram
- Maximum Score of a Good Subarray
- Asteroid Collision (Stack Simulation)
- Daily Temperatures

---

# Pattern

```text id="j4t3ke"
Monotonic Stack

Contribution Technique

Previous / Next Greater

Previous / Next Smaller
```

---

# Problem Statement

For every subarray,

compute

```text id="0by5sz"
Maximum - Minimum
```

Return the sum of these ranges over all subarrays.

---

# Brute Force

Generate every subarray.

Maintain

- current minimum
- current maximum

Add

```text id="m3xq0m"
max - min
```

to the answer.

---

## Complexity

```text id="tr9dc5"
Time  : O(n²)

Space : O(1)
```

---

# Key Observation

Instead of calculating the range for every subarray,

separate the expression.

```text id="4f34qn"
Range

=

Maximum

-

Minimum
```

Therefore,

```text id="rpf6ml"
Sum of Ranges

=

Sum of all Maximums

-

Sum of all Minimums
```

This transforms the problem into **two independent contribution problems**.

---

# Core Idea

For every element,

compute

```text id="i4z6mp"
How many subarrays
use this element
as the maximum?
```

and

```text id="8h2zw5"
How many subarrays
use this element
as the minimum?
```

Its contribution is

```text id="lljlwm"
value × number of subarrays
```

---

# Minimum Contribution

Find

```text id="zcawqk"
PSEE

Previous Smaller or Equal Element
```

and

```text id="j7lwjh"
NSE

Next Smaller Element
```

Then

```cpp id="dwyx1m"
left = i - psee[i];

right = nse[i] - i;

minimumContribution = left * right * arr[i];
```

---

# Maximum Contribution

Find

```text id="jl3xut"
PGEE

Previous Greater or Equal Element
```

and

```text id="4jgjw3"
NGE

Next Greater Element
```

Then

```cpp id="yj2m0o"
left = i - pgee[i];

right = nge[i] - i;

maximumContribution = left * right * arr[i];
```

---

# Final Formula

```cpp id="onznht"
answer

=

sumMaximums

-

sumMinimums
```

---

# Duplicate Handling

Exactly one side must use a strict comparison.

---

### Minimum

| Previous         | Next    |
| ---------------- | ------- |
| Smaller or Equal | Smaller |

Implementation

```cpp id="w7mxpq"
PSEE

pop while >

------------------

NSE

pop while >=
```

---

### Maximum

| Previous         | Next    |
| ---------------- | ------- |
| Greater or Equal | Greater |

Implementation

```cpp id="b2h38y"
PGEE

pop while <

------------------

NGE

pop while <=
```

This ensures every subarray is counted exactly once.

---

# Algorithm

```text id="gls9y8"
Find PSEE

↓

Find NSE

↓

Compute sum of minimums

--------------------

Find PGEE

↓

Find NGE

↓

Compute sum of maximums

--------------------

Answer

=

Maximum Sum

-

Minimum Sum
```

---

# Example

```text id="vzd6md"
nums

1 2 3
```

Subarrays

```text id="a4ljdw"
[1]

Range = 0

[2]

Range = 0

[3]

Range = 0

[1,2]

Range = 1

[2,3]

Range = 1

[1,2,3]

Range = 2
```

Total

```text id="ldkqk5"
4
```

Using contribution,

each element independently contributes as a maximum and as a minimum, and the difference gives the same result.

---

# Why It Works

Every subarray contributes

```text id="t2gzut"
Maximum

-

Minimum
```

Instead of visiting every subarray,

count

- how many subarrays each element is the maximum of,
- how many subarrays each element is the minimum of.

Since contribution is additive,

```text id="hkly3o"
Σ(Maximum)

-

Σ(Minimum)
```

is exactly the required answer.

---

# Complexity

```text id="gth7n8"
Time  : O(n)

Space : O(n)
```

Each monotonic stack pass is linear.

---

# Common Mistake #1

Trying to compute

```text id="g38cdy"
Maximum - Minimum
```

directly using one stack.

Maximums and minimums are independent contribution problems.

---

# Common Mistake #2

Using the same comparison for duplicates.

Wrong

```cpp id="dxfqg5"
>

>
```

or

```cpp id="7t8ocj"
>=

>=
```

One side must be strict, the other non-strict.

---

# Common Mistake #3

Using `int` for the accumulated answer.

Wrong

```cpp id="c95ncv"
int ans;
```

Correct

```cpp id="jlwm3n"
long long ans;
```

because

```text id="jruzwj"
left × right × value
```

can exceed 32-bit integer limits.

---

# Common Mistake #4

Forgetting that the final answer is

```text id="zqgp7l"
Maximum Sum

-

Minimum Sum
```

not the other way around.

---

# Comparison

| Problem                  | Contribution |           Stack Type |
| ------------------------ | -----------: | -------------------: |
| Sum of Subarray Minimums | Minimum only | Monotonic Increasing |
| Sum of Subarray Maximums | Maximum only | Monotonic Decreasing |
| Sum of Subarray Ranges   |         Both |                 Both |

---

# Pattern Recognition

Whenever you see

```text id="k8hzp6"
Sum of

(maximum - minimum)

over all subarrays
```

think

```text id="zb5k4x"
Split the expression

↓

Σ(Maximums)

-

Σ(Minimums)

↓

Solve each independently

↓

Contribution Technique

+

Monotonic Stack
```

---

# Interview Trigger

```text id="wbszqo"
Range = Max - Min

↓

Split into two sums

↓

Compute maximum contribution

↓

Compute minimum contribution

↓

Previous/Next Greater

+

Previous/Next Smaller

↓

Contribution = left × right × value

↓

Answer = MaxSum - MinSum

↓

O(n)
```

---

# Core Insight

The key observation is that **range is a linear expression**:

```text id="yfzoxb"
Maximum - Minimum
```

This allows the problem to be decomposed into two separate contribution problems. Using monotonic stacks, compute how many subarrays each element serves as the **maximum** and how many it serves as the **minimum**. The final answer is simply the difference between these two accumulated contributions, reducing the complexity from **O(n²)** to **O(n)**.
