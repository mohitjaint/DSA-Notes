# DP 09: Minimum Path Sum

## Problem

Given an `m × n` grid.

Start:

```txt id="mpsn01"
(0,0)
```

Destination:

```txt id="mpsn02"
(m-1,n-1)
```

Allowed moves:

```txt id="mpsn03"
Right
Down
```

Each cell contains a cost.

Find the **minimum path sum** from start to destination.

---

# State

```cpp id="mpsn04"
dp[i][j]
```

=

```txt id="mpsn05"
Minimum path sum from
cell (i,j)
to destination
```

---

# Choices

From cell `(i,j)`:

### Move Right

```cpp id="mpsn06"
dp[i][j+1]
```

---

### Move Down

```cpp id="mpsn07"
dp[i+1][j]
```

---

# Recurrence

Current cell must be included.

Therefore:

dp[i][j]=grid[i][j]+\min(dp[i+1][j],dp[i][j+1])

Meaning:

```txt id="mpsn08"
Current Cost
+
Best next path
```

---

# Base Case

Destination cell:

```cpp id="mpsn09"
dp[m-1][n-1]
=
grid[m-1][n-1]
```

Reason:

```txt id="mpsn10"
Already at destination.

Cost = value of destination cell.
```

---

# Tabulation Direction

Need:

```txt id="mpsn11"
Down
Right
```

Therefore:

```txt id="mpsn12"
Bottom-Right
→
Top-Left
```

---

# Space Optimization

## Observation

To compute current row:

```txt id="mpsn13"
Need:
Current row
Row below
```

Only one row is required.

---

## Compressed State

```cpp id="mpsn14"
vector<int> dp(n);
```

Meaning:

```txt id="mpsn15"
dp[j]
=
Minimum path sum from
current row, column j
to destination
```

---

# Transition

For a normal cell:

```cpp id="mpsn16"
dp[j] =
grid[i][j]
+
min(dp[j], dp[j+1]);
```

Interpretation:

```txt id="mpsn17"
dp[j]
=
Down

dp[j+1]
=
Right
```

Choose the cheaper path.

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

```txt id="mpsn18"
Grid DP
+
Min Cost DP
```

State:

```txt id="mpsn19"
(row, col)
```

Choices:

```txt id="mpsn20"
Right
Down
```

Combine:

```txt id="mpsn21"
min()
```

because we want the minimum total cost.

---

# Relation to Previous Problems

### Unique Paths

Recurrence:

dp[i][j]=dp[i+1][j]+dp[i][j+1]

Combine:

```txt id="mpsn22"
Addition
```

Count all paths.

---

### Unique Paths II

Recurrence:

```txt id="mpsn23"
Obstacle ? 0 : Down + Right
```

Combine:

```txt id="mpsn24"
Addition
```

Count valid paths.

---

### Minimum Path Sum

Recurrence:

dp[i][j]=grid[i][j]+\min(dp[i+1][j],dp[i][j+1])

Combine:

```txt id="mpsn25"
Minimum
```

Choose the cheapest path.

---

# General Grid DP Workflow

```txt id="mpsn26"
1. State = Cell

2. Identify allowed moves

3. Write recurrence

4. Decide combine operation:
   + / min / max

5. Fill according to dependencies

6. Compress rows if possible
```

---

# Key Takeaway

```txt id="mpsn27"
Most Grid DP problems
have the same state:

(row, col)

The main difference is
how answers from neighboring
cells are combined.
```

---

# Pattern Classification

```txt id="mpsn28"
Category:
Grid DP
+
Min Cost DP
```

Similar problems:

- Triangle
- Minimum Falling Path Sum
- Dungeon Game
- Cherry Pickup
- Various shortest-cost grid problems

---

# Main Insight

```txt id="mpsn29"
Unique Paths:
Count ways

Minimum Path Sum:
Choose cheapest way

Same state,
same moves,
different combine operation.
```
