# DP 04: House Robber

## Problem

Each house contains some money.

Constraint:

```txt id="hr01"
Cannot rob two adjacent houses.
```

Find the maximum money that can be robbed.

---

# State

```cpp id="hr02"
dp[i]
```

=

```txt id="hr03"
Maximum money obtainable
starting from house i
```

---

# Choices

At house `i`:

## Option 1: Skip

Move to next house.

```cpp id="hr04"
dp[i+1]
```

---

## Option 2: Rob

Take money from current house.

Must skip adjacent house.

```cpp id="hr05"
nums[i] + dp[i+2]
```

---

# Recurrence

dp[i]=\max(dp[i+1],\ nums[i]+dp[i+2])

---

# Base Cases

No houses left:

```cpp id="hr06"
dp[n] = 0
dp[n+1] = 0
```

These allow the recurrence to work uniformly.

---

# Memoization

```cpp id="hr07"
solve(i)
```

=

```txt id="hr08"
Maximum money obtainable
from house i onward
```

Store results to avoid recomputation.

---

# Tabulation

Since:

```txt id="hr09"
dp[i]
depends on

dp[i+1]
dp[i+2]
```

fill:

```txt id="hr10"
Right → Left
```

---

# Space Optimization

Observation:

```txt id="hr11"
Only dp[i+1]
and dp[i+2]
are needed.
```

Store:

```cpp id="hr12"
next1 = dp[i+1]
next2 = dp[i+2]
```

instead of the entire DP array.

---

# Space Optimized Code Logic

```cpp id="hr13"
curr = max(
    nums[i] + next2,
    next1
);
```

Update:

```cpp id="hr14"
next2 = next1;
next1 = curr;
```

---

# Complexity

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

# Pattern Recognition

This is the classic:

```txt id="hr15"
Pick / Not Pick DP
```

At every index:

```txt id="hr16"
Pick current element
OR
Skip current element
```

Choose the better option.

---

# Why Binary Mask Is Not Needed

Initial thought:

```txt id="hr17"
State = binary string
representing robbed houses
```

Number of states:

```txt id="hr18"
2^n
```

Too large.

---

Observation:

```txt id="hr19"
Future decisions only depend
on the current position.
```

Therefore:

```txt id="hr20"
State = index i
```

reduces complexity to:

```txt id="hr21"
O(n)
```

---

# General Form

Many DP problems follow:

```txt id="hr22"
At each position:

Pick
or
Don't Pick
```

Examples:

- House Robber
- Subset Sum
- Knapsack
- Target Sum
- Partition Problems

---

# Key Takeaway

```txt id="hr23"
If selecting an element
forces you to skip or restrict
future choices,

think:

Pick / Not Pick DP
```

---

# DP Workflow Used

```txt id="hr24"
1. Define State
2. Identify Choices
3. Write Recurrence
4. Add Base Cases
5. Convert to Tabulation
6. Optimize Space
```

This is the first major **Pick / Not Pick** DP pattern and is one of the most important foundations for future DP problems.
