# DP 05: House Robber II

## Problem

Houses are arranged in a **circle**.

Constraint:

```txt id="hr201"
Cannot rob two adjacent houses.
```

Since first and last houses are adjacent:

```txt id="hr202"
Cannot rob both
house 0 and house n-1.
```

---

# Key Observation

Every valid solution belongs to one of two cases:

### Case 1

Exclude the last house.

```txt id="hr203"
Consider houses:

[0 ... n-2]
```

---

### Case 2

Exclude the first house.

```txt id="hr204"
Consider houses:

[1 ... n-1]
```

---

Final answer:

\max(\text{rob}[0..n-2],\ \text{rob}[1..n-1])

---

# Reduction

House Robber II becomes:

```txt id="hr205"
Run House Robber I
twice.
```

---

# State

For each linear range:

```cpp id="hr206"
dp[i]
```

=

```txt id="hr207"
Maximum money obtainable
starting from house i
```

---

# Choices

At house `i`:

### Skip

```cpp id="hr208"
dp[i+1]
```

---

### Rob

```cpp id="hr209"
nums[i] + dp[i+2]
```

---

# Recurrence

dp[i]=\max(dp[i+1],\ nums[i]+dp[i+2])

---

# Tabulation Direction

Since:

```txt id="hr210"
dp[i]
depends on

dp[i+1]
dp[i+2]
```

fill:

```txt id="hr211"
Right → Left
```

---

# Space Optimization

Only need:

```txt id="hr212"
dp[i+1]
dp[i+2]
```

Store:

```cpp id="hr213"
next1 = dp[i+1]
next2 = dp[i+2]
```

---

# Your Optimization

Instead of creating:

```cpp id="hr214"
arr1 = [0...n-2]
arr2 = [1...n-1]
```

you directly run two loops:

### First Pass

```txt id="hr215"
Process houses

[0 ... n-2]
```

(last house excluded)

---

### Second Pass

```txt id="hr216"
Process houses

[1 ... n-1]
```

(first house excluded)

---

This avoids:

```txt id="hr217"
Copying arrays
```

and keeps:

```txt id="hr218"
Extra Space = O(1)
```

---

# Edge Case

```cpp id="hr219"
if(n == 1)
    return nums[0];
```

Reason:

```txt id="hr220"
With one house,
the circular constraint
does not matter.
```

---

# Complexity

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n)       |
| Space  | O(1)       |

---

# Pattern Recognition

Whenever you see:

```txt id="hr221"
Circular arrangement
+
Adjacent restriction
```

Think:

```txt id="hr222"
Break circle into
two linear cases.
```

---

# Common Examples

- House Robber II
- Circular Maximum Sum problems
- Circular DP/Greedy variants

---

# Key Takeaway

```txt id="hr223"
A circular problem is often solved by
breaking it into a small number
of linear subproblems.
```

For House Robber II:

```txt id="hr224"
Cannot take both
first and last.

⇒ Exclude first
OR
Exclude last.
```

Then solve each case using the standard **Pick / Not Pick DP** from House Robber I.
