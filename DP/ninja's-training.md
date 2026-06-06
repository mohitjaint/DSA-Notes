# DP 06: Ninja Training

## Problem

For each day, a ninja can perform one of:

```txt id="nt01"
Task 0
Task 1
Task 2
```

Each task gives some points.

Constraint:

```txt id="nt02"
Cannot perform the same task
on two consecutive days.
```

Find the maximum total points.

---

# State

```cpp id="nt03"
dp[i][j]
```

=

```txt id="nt04"
Maximum points obtainable
up to day i

if task j is performed on day i
```

where:

```txt id="nt05"
j = 0, 1, 2
```

---

# Why Previous Choice Matters

To decide today's task:

```txt id="nt06"
Need to know
which task was performed yesterday.
```

This is why the task index becomes part of the state.

---

# Choices

Suppose task `j` is performed today.

Yesterday's task can be:

```txt id="nt07"
Any task except j
```

---

# Recurrence

For every day `i` and task `j`:

dp[i][j]=points[i][j]+\max\_{k\neq j}(dp[i-1][k])

Meaning:

```txt id="nt08"
Today's points
+
Best valid task from previous day
```

---

# Base Case

First day:

```cpp id="nt09"
dp[0][0] = points[0][0]
dp[0][1] = points[0][1]
dp[0][2] = points[0][2]
```

or

```cpp id="nt10"
prev = points[0]
```

---

# Tabulation Direction

Since:

```txt id="nt11"
dp[i]
depends on
dp[i-1]
```

fill:

```txt id="nt12"
Top → Bottom
```

(day 0 → day n-1)

---

# Space Optimization

Observation:

```txt id="nt13"
Only previous day's values
are needed.
```

Need:

```cpp id="nt14"
prev[3]
curr[3]
```

instead of:

```cpp id="nt15"
dp[n][3]
```

---

# Transition

For each day:

```cpp id="nt16"
curr[j] =
max(
    points[i][j] + prev[k]
)
```

for all:

```txt id="nt17"
k ≠ j
```

After processing the day:

```cpp id="nt18"
prev = curr;
```

---

# Final Answer

After the last day:

```cpp id="nt19"
max(prev[0], prev[1], prev[2])
```

because the last task can be anything.

---

# Complexity

### Full DP Table

| Metric | Complexity   |
| ------ | ------------ |
| Time   | O(n × 3 × 3) |
| Space  | O(n × 3)     |

---

### Space Optimized

| Metric | Complexity          |
| ------ | ------------------- |
| Time   | O(n × 3 × 3) = O(n) |
| Space  | O(3) = O(1)         |

(3 tasks is constant.)

---

# Pattern Recognition

This is the first:

```txt id="nt20"
DP with Previous Choice
```

pattern.

State must remember:

```txt id="nt21"
What was chosen previously?
```

because current decisions depend on it.

---

# Similar Problems

- Paint House
- Paint Fence
- Stock DP
- Task Scheduling DP
- State Machine DP

---

# Key Takeaway

```txt id="nt22"
If current decisions depend on
a previous decision,

include that previous choice
in the DP state.
```

---

# DP Workflow Used

```txt id="nt23"
1. State:
   (day, task)

2. Choices:
   Any task except previous one

3. Recurrence:
   Current points + best valid previous task

4. Tabulation:
   Top → Bottom

5. Space Optimization:
   Store only previous day
```

---

# Main Insight

```txt id="nt24"
Unlike House Robber,

the state is not just the index.

We must also remember
which task was performed last.
```

This is often the first DP problem where the state has **multiple dimensions**:

```txt id="nt25"
(day, previous choice)
```

instead of just:

```txt id="nt26"
(index)
```
