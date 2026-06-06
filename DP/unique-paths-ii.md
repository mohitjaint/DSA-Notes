# DP 08: Unique Paths II

## Problem

Given an `m × n` grid.

Start:

```txt id="upi01"
(0,0)
```

Destination:

```txt id="upi02"
(m-1,n-1)
```

Allowed moves:

```txt id="upi03"
Right
Down
```

Some cells contain obstacles:

```txt id="upi04"
1 = obstacle
0 = free cell
```

Find the number of unique paths.

---

# State

```cpp id="upi05"
dp[i][j]
```

=

```txt id="upi06"
Number of paths from
cell (i,j)
to destination
```

---

# Choices

From a free cell:

### Move Right

```cpp id="upi07"
dp[i][j+1]
```

---

### Move Down

```cpp id="upi08"
dp[i+1][j]
```

---

# Recurrence

For a free cell:

dp[i][j]=dp[i+1][j]+dp[i][j+1]

---

For an obstacle:

```cpp id="upi09"
dp[i][j] = 0;
```

Reason:

```txt id="upi10"
No path can pass
through an obstacle.
```

---

# Base Case

Destination cell:

```cpp id="upi11"
dp[m-1][n-1]
=
0 if blocked
1 otherwise
```

Initialization:

```cpp id="upi12"
dp[n-1] =
obstacleGrid[m-1][n-1]
? 0
: 1;
```

---

# Tabulation Direction

Need:

```txt id="upi13"
Down
Right
```

Therefore:

```txt id="upi14"
Bottom-Right
→
Top-Left
```

---

# Space Optimization

## Observation

To compute current row:

```txt id="upi15"
Need:
Current row
Row below
```

Only one row is required.

---

## Compressed State

```cpp id="upi16"
vector<int> dp(n);
```

Meaning:

```txt id="upi17"
dp[j]
=
Number of paths from
current row, column j
to destination
```

---

# Transition

```cpp id="upi18"
if(obstacleGrid[i][j])
    dp[j] = 0;
else
    dp[j] += dp[j+1];
```

Interpretation:

```txt id="upi19"
dp[j]
=
Down + Right
```

where:

```txt id="upi20"
dp[j]
=
paths from below

dp[j+1]
=
paths from right
```

---

# Complexity

### Full DP Table

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(m × n)   |
| Space  | O(m × n)   |

---

### Space Optimized

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(m × n)   |
| Space  | O(n)       |

---

# Pattern Recognition

```txt id="upi21"
Grid DP
+
Blocked Cells
```

State:

```txt id="upi22"
(row, col)
```

Choices:

```txt id="upi23"
Right
Down
```

Combine:

```txt id="upi24"
Addition
```

because we are counting all valid paths.

---

# Difference from Unique Paths I

### Unique Paths I

```txt id="upi25"
Every cell contributes paths.
```

Recurrence:

```txt id="upi26"
Down + Right
```

---

### Unique Paths II

```txt id="upi27"
Obstacle cells contribute 0.
```

Recurrence:

```txt id="upi28"
Obstacle ? 0 : Down + Right
```

---

# General Grid DP Workflow

```txt id="upi29"
1. State = Cell

2. Identify allowed moves

3. Write recurrence

4. Handle blocked cells
   by assigning 0

5. Fill according to
   dependency direction

6. Compress rows if possible
```

---

# Key Takeaway

```txt id="upi30"
In path-counting problems,

Obstacle Cell
=
0 ways

Everything else usually
follows the same recurrence.
```

---

# Pattern Classification

```txt id="upi31"
Category:
Grid DP
+
Count Ways DP
```

Similar problems:

- Unique Paths
- Grid Path Counting
- Maze Path Counting
- Cherry Pickup (advanced)
- Dungeon Game (advanced)

---

# Main Insight

```txt id="upi32"
Adding obstacles does not
change the DP structure.

It only changes the value
of blocked cells to 0.
```
