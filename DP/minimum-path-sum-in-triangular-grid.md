# DP 10: Triangle

## Problem

Given a triangle:

```txt
    2
   3 4
  6 5 7
 4 1 8 3
```

Start from the top.

From cell `(i,j)` you can move to:

```txt
(i+1, j)
(i+1, j+1)
```

Find the **minimum path sum** from top to bottom.

---

# State

```cpp
dp[i][j]
```

=

```txt
Minimum path sum from
triangle[i][j]
to the bottom.
```

---

# Choices

From a cell:

### Down

```cpp
dp[i+1][j]
```

---

### Diagonal Down-Right

```cpp
dp[i+1][j+1]
```

---

# Recurrence

dp[i][j]=triangle[i][j]+\min(dp[i+1][j],dp[i+1][j+1])

Meaning:

```txt
Current Value
+
Best path among the two possible moves
```

---

# Base Case

Last row:

```cpp
dp[n-1][j] = triangle[n-1][j]
```

Reason:

```txt
Already at the bottom.

Minimum cost = cell value itself.
```

---

# Tabulation Direction

Need:

```txt
dp[i+1][j]
dp[i+1][j+1]
```

Therefore:

```txt
Bottom → Top
```

---

# Basic DP Solution

```cpp
vector<vector<int>> dp(n, vector<int>(n));

dp[n-1] = triangle[n-1];

for(int i = n-2; i >= 0; i--){
    for(int j = 0; j <= i; j++){
        dp[i][j] =
            triangle[i][j] +
            min(dp[i+1][j],
                dp[i+1][j+1]);
    }
}
```

---

# Space Optimization

## Observation

```txt
Current row depends only
on the row below.
```

No need to store all rows.

---

## Compressed State

```cpp
vector<int> dp = triangle[n-1];
```

Meaning:

```txt
dp[j]
=
Minimum path sum from
the cell below/current row
to the bottom.
```

---

# Transition

```cpp
dp[j] =
triangle[i][j]
+
min(dp[j], dp[j+1]);
```

Where:

```txt
dp[j]
=
Down

dp[j+1]
=
Diagonal Down-Right
```

---

# Why In-Place Update Works

While processing row `i`:

```txt
dp[j]
dp[j+1]
```

still contain answers from row `i+1`.

Therefore:

```cpp
dp[j] =
triangle[i][j]
+
min(dp[j], dp[j+1]);
```

can safely overwrite `dp[j]`.

---

# Complexity

### Full DP

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n²)      |
| Space  | O(n²)      |

---

### Space Optimized

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n²)      |
| Space  | O(n)       |

---

# Pattern Recognition

```txt
Grid DP
+
Min Cost DP
```

except the grid is triangular.

---

# Relation to Previous Problems

### Unique Paths

```txt
Current Cell
=
Right + Down
```

Count all paths.

---

### Unique Paths II

```txt
Obstacle ? 0 : Right + Down
```

Count valid paths.

---

### Minimum Path Sum

```txt
Current Cell
=
Cell Value + min(Right, Down)
```

Choose cheapest path.

---

### Triangle

```txt
Current Cell
=
Cell Value + min(Down, Diagonal)
```

Choose cheapest path.

---

# General Grid DP Workflow

```txt
1. State = Cell

2. Identify valid moves

3. Write recurrence

4. Decide combine operation:
   + / min / max

5. Fill according to dependencies

6. Compress rows if only the next row is needed
```

---

# Key Takeaway

```txt
When a DP state depends only
on the next row,

the DP table can usually be
compressed into a single row.
```

---

# Pattern Classification

```txt
Category:
Grid DP
+
Min Cost DP
```

Similar problems:

- Minimum Path Sum
- Minimum Falling Path Sum
- Dungeon Game
- Cherry Pickup
- Various shortest-cost grid problems

---

# Main Insight

```txt
The triangle shape does not change
the DP approach.

Only the available moves change:

Down
Diagonal Down-Right
```

The state and optimization ideas remain exactly the same as other Grid DP problems.
