# DP 11: Cherry Pickup II / Ninja and Friends

## Problem

Two robots start at:

```txt
Alice : (0,0)
Bob   : (0,n-1)
```

Both move row by row until the last row.

At each step:

```txt
Column - 1
Column
Column + 1
```

are allowed.

Collect maximum chocolates.

If both robots reach the same cell:

```txt
Count chocolates only once.
```

---

# Key Observation

At any moment, the future depends on:

```txt
Current Row
Alice Column
Bob Column
```

Therefore one index is not enough.

---

# State

```cpp
dp[i][j1][j2]
```

=

```txt
Maximum chocolates collectable
from row i to the last row

Alice at column j1
Bob at column j2
```

---

# Why 3D DP?

Need to know:

```txt
Which row we are in
Where Alice is
Where Bob is
```

Therefore:

```cpp
(row, aliceCol, bobCol)
```

is the minimum valid state.

---

# Choices

Alice:

```txt
j1-1
j1
j1+1
```

3 possibilities.

---

Bob:

```txt
j2-1
j2
j2+1
```

3 possibilities.

---

Total transitions:

```txt
3 × 3 = 9
```

---

# Current Row Contribution

If both robots stand on different cells:

```cpp
grid[i][j1] + grid[i][j2]
```

---

If both stand on the same cell:

```cpp
grid[i][j1]
```

Count only once.

---

# Recurrence

Let:

```cpp
val
```

be chocolates collected in current row.

Then:

```txt
Answer =
Current Chocolates
+
Best among all 9 future states
```

Mathematically:

```cpp
dp[i][j1][j2]
=
val
+
max(
    dp[i+1][j1+dj1][j2+dj2]
)
```

for all valid:

```txt
dj1 ∈ {-1,0,1}
dj2 ∈ {-1,0,1}
```

---

# Base Case

Last row:

If:

```cpp
j1 == j2
```

```cpp
dp[last][j1][j2]
=
grid[last][j1]
```

---

Otherwise:

```cpp
dp[last][j1][j2]
=
grid[last][j1]
+
grid[last][j2]
```

---

# Tabulation Direction

Need:

```txt
Row i+1
```

to compute:

```txt
Row i
```

Therefore:

```txt
Bottom → Top
```

---

# Full DP

```cpp
dp[m][n][n]
```

---

## Complexity

States:

```txt
m × n × n
```

Transitions per state:

```txt
9
```

Therefore:

```txt
Time  = O(m × n² × 9)
      = O(m × n²)

Space = O(m × n²)
```

---

# Space Optimization

## Observation

Current row depends only on:

```txt
Next row
```

Never uses:

```txt
Row i+2
Row i+3
...
```

---

Therefore:

```cpp
dp[m][n][n]
```

can be compressed to:

```cpp
prev[n][n]
curr[n][n]
```

---

# Meaning of Optimized State

```cpp
prev[j1][j2]
```

=

```txt
Maximum chocolates collectable
from next row onwards

Alice at j1
Bob at j2
```

---

```cpp
curr[j1][j2]
```

=

```txt
Maximum chocolates collectable
from current row onwards

Alice at j1
Bob at j2
```

---

After finishing a row:

```cpp
prev = curr;
```

---

# Optimized Complexity

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(m × n²)  |
| Space  | O(n²)      |

---

# Pattern Recognition

This is the first classic:

```txt
Multi-Agent DP
```

where multiple entities move simultaneously.

---

# Common DP Evolution

### Single Position

```cpp
dp[i]
```

Examples:

```txt
Climbing Stairs
House Robber
Frog Jump
```

---

### Grid Position

```cpp
dp[i][j]
```

Examples:

```txt
Unique Paths
Minimum Path Sum
Triangle
```

---

### Multiple Positions

```cpp
dp[i][j1][j2]
```

Examples:

```txt
Cherry Pickup II
Ninja and Friends
```

---

# Key Takeaway

```txt
DP dimensions are determined
by the amount of information
needed to uniquely describe
the current situation.
```

For this problem:

```txt
Current Row
Alice Position
Bob Position
```

Therefore:

```cpp
dp[row][alice][bob]
```

is unavoidable.

---

# Main Insight

```txt
Don't think:

"3D Array"

Think:

"What information is required
to describe the current state?"
```

Here the state is:

```txt
(Row, Alice Position, Bob Position)
```

and the DP naturally becomes 3-dimensional.
