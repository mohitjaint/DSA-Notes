# Online Stock Span (LeetCode 901)

## References

**Question:**

[https://leetcode.com/problems/online-stock-span/](https://leetcode.com/problems/online-stock-span/)

**Related Problems:**

- Next Greater Element
- Daily Temperatures
- Largest Rectangle in Histogram
- Sliding Window Maximum
- Asteroid Collision

---

# Pattern

```text
Monotonic Decreasing Stack

Previous Greater Element (PGE)

Online Queries
```

---

# Problem Statement

Design a data structure that receives stock prices one by one.

For each incoming price,

return the **stock span**.

The span is the number of consecutive previous days (including today) whose prices are **less than or equal** to today's price.

---

# Brute Force

For every new price,

move backwards until a greater price is found.

Count the number of valid days.

---

## Complexity

```text
Time  : O(n) per query

Space : O(1)
```

For `n` queries,

```text
Total Time : O(n²)
```

---

# Key Observation

Suppose today's price is

```text
75
```

Previous prices

```text
100 80 60 70 60 75
```

The span stops at the first previous price that is **strictly greater**.

Therefore,

we only need the

```text
Previous Greater Element (PGE)
```

---

# Core Idea

Let

```text
PGE = index of previous greater price
```

Then every day between

```text
PGE + 1
```

and

```text
Today
```

has price ≤ today's price.

Therefore

```text
Span = CurrentIndex - PGE
```

If no greater element exists,

```text
PGE = -1
```

and

```text
Span = CurrentIndex + 1
```

---

# Monotonic Stack

Maintain a **decreasing stack**.

Each element stores

```text
(price, index)
```

For every new price

Remove every previous price that is

```text
<= Current Price
```

because it can never become the Previous Greater Element for the current or any future larger price.

---

# Algorithm

```text
Receive new price

↓

Pop all prices <= current price

↓

Stack top = Previous Greater Element

↓

Span = CurrentIndex - PGE

↓

Push current (price, index)

↓

Return span
```

---

# Example

Prices

```text
100 80 60 70 60 75 85
```

---

Day 0

```text
100

Stack

100

Span = 1
```

---

Day 1

```text
80

Previous Greater = 100

Span = 1
```

---

Day 2

```text
60

Previous Greater = 80

Span = 1
```

---

Day 3

```text
70
```

Pop

```text
60
```

Previous Greater

```text
80
```

Span

```text
2
```

---

Day 5

```text
75
```

Pop

```text
60

70
```

Previous Greater

```text
80
```

Span

```text
4
```

---

Day 6

```text
85
```

Pop

```text
75

80
```

Previous Greater

```text
100
```

Span

```text
6
```

Answer

```text
1 1 1 2 1 4 6
```

---

# Why Does This Work?

The stack always stores prices in **strictly decreasing order**.

Whenever a new price arrives,

every smaller (or equal) price is removed because

- it cannot be the Previous Greater Element of the current price.
- it also cannot be the Previous Greater Element of any future price that is at least as large as the current one.

Thus, after popping,

the stack top is always the nearest previous greater price.

---

# Complexity

Each price

- is pushed exactly once.
- is popped at most once.

Therefore

```text
Time  : O(1) amortized per query

Total : O(n)

Space : O(n)
```

---

# Amortized Analysis

Although one query may pop many elements,

those elements are removed permanently.

Example

```text
100
90
80
70
60
50
40
30
20
10
```

Now insert

```text
200
```

This query pops every previous element.

It takes

```text
O(n)
```

But those elements will **never be popped again**.

Across all queries

```text
Total Pushes ≤ n

Total Pops ≤ n
```

So the total work is

```text
2n
```

Therefore,

```text
Average Cost Per Query

=

O(1)
```

This is called **amortized O(1)** time.

---

# Optimized Variant (Price + Span)

Instead of storing

```text
(price, index)
```

store

```text
(price, span)
```

When a smaller price is removed,

its previously computed span can be reused.

```cpp
span = 1;

while(!st.empty() && st.top().first <= price){
    span += st.top().second;
    st.pop();
}

st.push({price, span});
```

No indices are required.

---

# Why Does the (Price, Span) Variant Work?

Suppose the stack contains

```text
(100,1)

(80,1)

(70,2)

(60,1)
```

Current price

```text
75
```

Remove

```text
(60,1)
```

Current span becomes

```text
2
```

Remove

```text
(70,2)
```

Current span becomes

```text
4
```

Push

```text
(75,4)
```

The span of removed prices has already been computed, so we simply reuse it.

---

# Comparison

| Approach    | Stack Stores   | Time per Query | Space |
| ----------- | -------------- | -------------: | ----: |
| Brute Force | None           |           O(n) |  O(1) |
| PGE (Index) | (Price, Index) | O(1) amortized |  O(n) |
| Optimized   | (Price, Span)  | O(1) amortized |  O(n) |

---

# Common Mistake #1

Using `<` instead of `<=`

Correct

```cpp
while(!st.empty() && st.top().first <= price)
```

Equal prices also contribute to the span.

---

# Common Mistake #2

Confusing Previous Greater with Previous Greater or Equal.

The span stops only at a **strictly greater** price.

Therefore we remove every price that is **less than or equal** to the current price.

---

# Common Mistake #3

Thinking the algorithm is `O(n²)`.

Although one query may pop many elements,

each element is popped only once.

The complexity is **O(1) amortized** per query.

---

# Pattern Recognition

Whenever you see

```text
Online queries

↓

Need nearest previous greater/smaller

↓

Each element becomes useless after a future comparison
```

think

```text
Monotonic Stack
```

If the answer depends only on previous elements and each query arrives one at a time, a monotonic stack is often the right choice.

---

# Interview Trigger

```text
Online stock prices

↓

Need consecutive previous prices ≤ current

↓

Find Previous Greater Element

↓

Maintain decreasing stack

↓

Pop all prices ≤ current

↓

Stack top = PGE

↓

Span = CurrentIndex − PGE

↓

Push current price

↓

O(1) amortized
```

---

# Core Insight

The stock span for today's price depends only on the **nearest previous price that is strictly greater**. A monotonic decreasing stack efficiently maintains these candidates by discarding prices that can never serve as a Previous Greater Element again. Since every price is pushed once and popped at most once, the total work across all queries is linear, giving **O(1) amortized** time per `next()` call. The `(price, span)` variant further improves elegance by reusing previously computed spans instead of storing indices.
