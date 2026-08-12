# Lemonade Change — LeetCode 860

## Reference

**Question:** [https://leetcode.com/problems/lemonade-change/](https://leetcode.com/problems/lemonade-change/)

**Pattern:**
`Greedy + Frequency Counting`

---

# Problem

You sell lemonade for `$5`.

Customers pay with:

```text
$5
$10
$20
```

For every customer, provide the correct change.

Return `false` if at any point you cannot provide the required change.

---

# Key Observation

We only need to track:

```cpp id="4kz8vw"
freq5
freq10
```

We don't need to track `$20` bills because:

> A `$20` bill can never be used to give change to a future customer.

---

# Greedy Insight

When receiving a `$20` bill, the required change is:

```text id="v8lqjr"
$15
```

There are two possibilities:

```text id="x9v2qf"
$10 + $5
```

or

```text id="b7k3q1"
$5 + $5 + $5
```

**Always prefer `$10 + $5`.**

Why?

Because `$5` bills are more useful for future transactions.

A `$5` can be used to make change for:

```text id="r2q4cz"
$10 → $5
$20 → $15
```

while a `$10` can only help with:

```text id="c7j8mw"
$20 → $10 + $5
```

Therefore:

> **Preserve `$5` bills whenever possible.**

---

# Case 1 — Customer Pays $5

Required change:

```text id="2d4qk7"
$0
```

Simply keep the bill.

```cpp id="h8f3qp"
freq5++;
```

---

# Case 2 — Customer Pays $10

Required change:

```text id="m7z4px"
$5
```

Need one `$5`.

```cpp id="f1j8wc"
if(freq5 > 0){
    freq5--;
    freq10++;
}
else
    return false;
```

---

# Case 3 — Customer Pays $20

Required change:

```text id="s5y2nr"
$15
```

### Preferred option

```text id="7r3mqa"
$10 + $5
```

```cpp id="k4t8vz"
if(freq5 > 0 && freq10 > 0){
    freq5--;
    freq10--;
}
```

---

### Alternative

If `$10 + $5` isn't available:

```text id="x8n2pf"
$5 + $5 + $5
```

```cpp id="q5v7kd"
else if(freq5 >= 3){
    freq5 -= 3;
}
```

---

### Otherwise

Cannot provide `$15`.

```cpp id="r6m1zs"
return false;
```

---

# Algorithm

```text id="c4n8ty"
Initialize:

freq5 = 0
freq10 = 0

For every bill:

    $5:
        freq5++

    $10:
        need one $5
        if available:
            freq5--
            freq10++
        else:
            return false

    $20:
        need $15

        Prefer:
            $10 + $5

        Otherwise:
            $5 + $5 + $5

        Otherwise:
            return false

Return true
```

---

# Example

```text id="w8k2pz"
bills = [5, 5, 5, 10, 20]
```

Start:

```text id="r3m7xa"
$5 = 0
$10 = 0
```

### `$5`

```text id="q7n4bw"
$5 = 1
```

### `$5`

```text id="q1s9ce"
$5 = 2
```

### `$5`

```text id="n6v3rk"
$5 = 3
```

### `$10`

Give one `$5`:

```text id="f8j2md"
$5 = 2
$10 = 1
```

### `$20`

Need `$15`.

Use:

```text id="z4c6pq"
$10 + $5
```

Remaining:

```text id="j2w7ns"
$5 = 1
$10 = 0
```

Successfully served everyone.

---

# Why the Greedy Choice Works

The critical decision is when giving `$15` change for a `$20`.

Suppose we have:

```text id="x4p9qa"
$5 = 3
$10 = 1
```

Using three `$5` bills would leave:

```text id="d7m2kc"
$5 = 0
$10 = 1
```

Using `$10 + $5` leaves:

```text id="b5q8vz"
$5 = 2
$10 = 0
```

The second state is better because `$5` bills are more flexible.

Thus:

```text id="k3x7ma"
$10 + $5
```

is the optimal greedy choice whenever available.

---

# Complexity

For `n = bills.size()`:

```text id="f2q9wd"
Time  : O(n)

Space : O(1)
```

Each customer is processed exactly once.

---

# Common Mistake

### Using `$5 + $5 + $5` first

Wrong greedy choice:

```text id="m4x8rs"
if(freq5 >= 3)
```

before checking for `$10 + $5`.

This consumes three highly useful `$5` bills unnecessarily.

Correct priority:

```text id="a7v2kc"
$10 + $5

        ↓ if unavailable

$5 + $5 + $5
```

---

# Pattern Recognition

When a problem involves:

```text id="q9s4mt"
Sequential decisions

+

Limited resources

+

Current choice affects future feasibility

+

Need to maximize the chance of future success
```

look for a **Greedy strategy**.

Here:

```text id="x2k8vn"
Resource = $5 bills

↓

Preserve the more flexible resource
```

---

# Interview Trigger

```text id="m8c3zp"
Lemonade Change

↓

Track $5 and $10

↓

$5 → keep it

↓

$10 → give $5

↓

$20 → need $15

↓

Prefer $10 + $5

↓

Otherwise 3 × $5

↓

If neither possible → false

↓

O(n)
```

---

# Core Insight

The entire problem revolves around **preserving `$5` bills**. A `$20` customer can be given change either as `$10 + $5` or `3 × $5`. The first option is greedily optimal because it uses fewer `$5` bills, leaving more of the flexible `$5` resource available for future `$10` and `$20` customers. Thus, the optimal strategy is to **use `$10 + $5` whenever possible and fall back to three `$5`s only when necessary**.
