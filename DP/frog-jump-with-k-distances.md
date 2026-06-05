# DP 03: Frog Jump with K Distance

## Problem

A frog starts at stone `0` and wants to reach stone `n-1`.

Allowed jumps:

```txt id="dpk01"
1 step
2 steps
...
k steps
```

Jump cost:

```cpp id="dpk02"
abs(height[a] - height[b])
```

Find the minimum energy required.

---

# State

```cpp id="dpk03"
dp[i]
```

=

```txt id="dpk04"
Minimum energy required
to reach stone i
```

---

# Choices

To reach stone `i`, the frog can come from:

```txt id="dpk05"
i-1
i-2
...
i-k
```

(if valid)

---

# Recurrence

dp[i]=\min*{1\le j\le k}(dp[i-j]+|h_i-h*{i-j}|)

Interpretation:

```txt id="dpk06"
Try every possible jump length.

Take the minimum cost among them.
```

---

# Base Case

```cpp id="dpk07"
dp[0] = 0;
```

Reason:

```txt id="dpk08"
Already standing on stone 0.
No energy needed.
```

---

# Memoization

```cpp id="dpk09"
solve(i)
```

=

```txt id="dpk10"
Minimum energy required
to reach stone i.
```

Try all previous `k` stones.

Store results.

---

# Tabulation Direction

State:

```txt id="dpk11"
dp[i]
depends on

dp[i-1]
dp[i-2]
...
dp[i-k]
```

Therefore:

```txt id="dpk12"
Fill Left → Right
```

because all dependencies lie behind.

---

# Standard Tabulation

```cpp id="dpk13"
for(int i = 1; i < n; i++){

    for(int j = 1; j <= k; j++){

        if(i-j < 0) break;

        dp[i] = min(
            dp[i],
            dp[i-j]
            + abs(h[i]-h[i-j])
        );
    }
}
```

---

# Space Optimization

## Observation

To compute:

```cpp id="dpk14"
dp[i]
```

we only need:

```txt id="dpk15"
dp[i-1]
dp[i-2]
...
dp[i-k]
```

Older states are never used again.

---

## Your Optimization

Maintain a sliding window:

```cpp id="dpk16"
minusX[1] = dp[i-1]
minusX[2] = dp[i-2]
...
minusX[k] = dp[i-k]
```

Shift the window after every iteration.

This reduces space from:

```txt id="dpk17"
O(n)
```

to:

```txt id="dpk18"
O(k)
```

---

# Complexity

### Standard DP

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n·k)     |
| Space  | O(n)       |

---

### Sliding Window Optimization

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n·k)     |
| Space  | O(k)       |

---

# Pattern Recognition

Whenever you see:

```txt id="dpk19"
Minimum cost
+
Multiple previous choices
```

Think:

```txt id="dpk20"
DP with transition over
last K states
```

---

# General DP Pattern

Many problems follow:

dp[i]=\text{best among previous K states}

Examples:

- Frog Jump K Distance
- K-Step Stair Climbing
- Some Stock DP transitions
- Sliding-window DP problems

---

# Key Takeaway

```txt id="dpk21"
1. Define state.
2. Enumerate all valid previous states.
3. Write recurrence.
4. Determine dependency direction.
5. Check if only a fixed-size window of states is needed.
```

---

# Interview Insight

Most candidates stop at:

```txt id="dpk22"
O(n·k) time
O(n) space
```

A useful follow-up observation is:

```txt id="dpk23"
Only last K DP states are required.

⇒ Space can be reduced to O(k).
```

That's exactly the optimization you discovered on your own.
