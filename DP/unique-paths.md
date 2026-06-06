# DP 07: Unique Paths

## Problem

Given an `m × n` grid.

Start:

```txt id="unp01"
(0,0)
```

Destination:

```txt id="unp02"
(m-1,n-1)
```

Allowed moves:

```txt id="unp03"
Right
Down
```

Find the number of unique paths.

---

# State

```cpp id="unp04"
dp[i][j]
```

=

```txt id="unp05"
Number of paths from
cell (i,j)
to destination
```

---

# Choices

From cell `(i,j)`:

### Move Right

```cpp id="unp06"
dp[i][j+1]
```

---

### Move Down

```cpp id="unp07"
dp[i+1][j]
```

---

# Recurrence

dp[i][j]=dp[i][j+1]+dp[i+1][j]

Reason:

```txt id="unp08"
Total paths from current cell
=
Paths going right
+
Paths going down
```

---

# Base Case

Destination cell:

```cpp id="unp09"
dp[m-1][n-1] = 1
```

Reason:

```txt id="unp10"
Already at destination.

Exactly one valid path.
```

---

# Memoization

```cpp id="unp11"
solve(i,j)
```

=

```txt id="unp12"
Number of paths from
(i,j)
to destination
```

Store results to avoid recomputation.

---

# Tabulation

Since:

```txt id="unp13"
dp[i][j]
depends on

dp[i][j+1]
dp[i+1][j]
```

fill:

```txt id="unp14"
Bottom-Right
→
Top-Left
```

---

# Space Optimization

## Observation

To compute a row:

```txt id="unp15"
Need:
Current row
Row below
```

Only one row needs to be stored.

---

## State Compression

Use:

```cpp id="unp16"
vector<int> dp(n,1);
```

Initial meaning:

```txt id="unp17"
Last row
```

Example:

```txt id="unp18"
1 1 1 1 1
```

because from the last row:

```txt id="unp19"
Can only move right.
```

---

# Transition

```cpp id="unp20"
dp[j] = dp[j] + dp[j+1];
```

Interpretation:

```txt id="unp21"
dp[j]
=
paths from below

dp[j+1]
=
paths from right
```

Therefore:

```txt id="unp22"
Current cell
=
Down + Right
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

This is the first classic:

```txt id="unp23"
Grid DP
```

State:

```txt id="unp24"
(row, col)
```

Choices:

```txt id="unp25"
Right
Down
```

Combine:

```txt id="unp26"
Addition
```

because we are counting all valid paths.

---

# General Grid DP Workflow

```txt id="unp27"
1. Define state as a cell

2. Identify allowed moves

3. Write recurrence using reachable cells

4. Fill according to dependency direction

5. Compress rows if only adjacent rows are needed
```

---

# Pattern Classification

```txt id="unp28"
Category:
Count Ways DP
```

Similar problems:

- Climbing Stairs
- Unique Paths II
- Grid Path Counting
- Maze Path Counting

---

# Key Takeaway

```txt id="unp29"
When counting paths,

Answer at a cell
=
Sum of answers
from all valid next cells.
```

---

# Main Insight

```txt id="unp30"
Grid DP is just DP on coordinates.

State:
(row, col)

instead of

(index)
```

This is the foundation for:

- Unique Paths II
- Minimum Path Sum
- Triangle
- Dungeon Game
- Cherry Pickup
- Many advanced 2D DP problems.
