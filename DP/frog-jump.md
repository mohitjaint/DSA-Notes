# DP 02: Frog Jump

## Problem

A frog starts at stone `0` and wants to reach stone `n-1`.

Allowed jumps:

```txt id="m61z1g"
1 step
or
2 steps
```

Cost of a jump:

```cpp id="7yec1h"
abs(height[a] - height[b])
```

Find the minimum total energy required.

---

# State Definition

```cpp id="52m66o"
dp[i]
```

=

```txt id="z9kjft"
Minimum energy needed
to reach the last stone
starting from stone i
```

---

# Choices

From stone `i`:

### Jump 1 Step

```cpp id="rcddkh"
abs(h[i]-h[i+1]) + dp[i+1]
```

---

### Jump 2 Steps

```cpp id="njx9t5"
abs(h[i]-h[i+2]) + dp[i+2]
```

---

# Recurrence

dp[i]=\min(|h*i-h*{i+1}|+dp[i+1],\ |h*i-h*{i+2}|+dp[i+2])

---

# Base Cases

Already at destination:

```cpp id="6y1rmt"
dp[n-1] = 0
```

One jump away:

```cpp id="8wjm20"
dp[n-2] =
abs(h[n-2]-h[n-1])
```

---

# Memoization

```cpp id="sxvlzv"
solve(i)
```

returns:

```txt id="7pn8v9"
minimum energy from i to end
```

Store answers to avoid recomputation.

---

# Tabulation

Since:

```txt id="z2prph"
dp[i]
depends on
dp[i+1], dp[i+2]
```

fill:

```txt id="i6m2h6"
right → left
```

---

# Space Optimization

Observation:

```txt id="tkhvsf"
Only dp[i+1]
and dp[i+2]
are needed.
```

Store:

```cpp id="7s04ls"
next1 = dp[i+1]
next2 = dp[i+2]
```

instead of entire DP array.

---

# Update

```cpp id="e7txon"
curr = min(jump1, jump2);

next2 = next1;
next1 = curr;
```

---

# Complexity

### Memoization

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n)       |
| Space  | O(n)       |

---

### Tabulation

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n)       |
| Space  | O(n)       |

---

### Space Optimized

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n)       |
| Space  | O(1)       |

---

# Important DP Insight

There can be multiple valid state definitions.

### Forward DP

```cpp id="8d5l4e"
dp[i]
=
minimum cost to reach i
```

Fill:

```txt id="k2j6o7"
left → right
```

---

### Backward DP

```cpp id="a7m6fh"
dp[i]
=
minimum cost from i to reach end
```

Fill:

```txt id="1xq49z"
right → left
```

Both are correct.

---

# DP Workflow

```txt id="8qj8zl"
State
→ Choices
→ Recurrence
→ Base Cases
→ Memoization
→ Tabulation
→ Space Optimization
```

---

# Pattern Recognition

Whenever you see:

```txt id="6xyqpk"
Minimum/Maximum cost
+
Few future choices
```

Think:

```txt id="ch74a8"
DP with min/max recurrence
```

---

# Takeaway

```txt id="mjlwm2"
Tabulation direction
depends on the recurrence.

Fill states only after
all dependent states
are already computed.
```
