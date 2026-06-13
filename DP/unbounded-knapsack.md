# Unbounded Knapsack

## References

Question:

[https://www.geeksforgeeks.org/problems/knapsack-with-duplicate-items4201/1](https://www.geeksforgeeks.org/problems/knapsack-with-duplicate-items4201/1)

Related Problems:

[https://leetcode.com/problems/coin-change/](https://leetcode.com/problems/coin-change/)

[https://leetcode.com/problems/coin-change-ii/](https://leetcode.com/problems/coin-change-ii/)

[https://www.geeksforgeeks.org/problems/0-1-knapsack-problem0945/1](https://www.geeksforgeeks.org/problems/0-1-knapsack-problem0945/1)

[https://www.geeksforgeeks.org/problems/rod-cutting0840/1](https://www.geeksforgeeks.org/problems/rod-cutting0840/1)

---

# Pattern

```txt
Unbounded Knapsack

Maximize Profit

Unlimited Reuse Allowed

DP on Capacity
```

---

# Key Observation

Unlike 0/1 Knapsack:

```txt
Take item at most once
```

here:

```txt
Take item unlimited times
```

Therefore after taking an item:

```txt
Item remains available
```

This changes the transition completely.

---

# Constraint Analysis

```txt
Capacity W ≤ 1000

Items ≤ 1000
```

Capacity is manageable.

Therefore:

```txt
DP on Capacity ✓
```

---

# Why Greedy Fails

Example:

```txt
Weight = [5,4]

Value  = [10,9]

Capacity = 8
```

Greedy by value:

```txt
Take weight 5
Profit = 10
```

Remaining:

```txt
3
```

Total:

```txt
10
```

Optimal:

```txt
4 + 4
```

Profit:

```txt
18
```

Therefore:

```txt
Greedy ✗
DP ✓
```

---

# Bruteforce

For every item:

```txt
Take 0 times
Take 1 time
Take 2 times
...
```

Recursively try all possibilities.

Complexity:

```txt
Exponential
```

Too large.

---

# State

```cpp
dp[i][capacity]
```

Meaning:

```txt
Maximum profit obtainable

using items [0...i]

with remaining capacity = capacity
```

---

# Base Case

Only first item available:

```cpp
i = 0
```

Best strategy:

```txt
Take it as many times as possible
```

Therefore:

```cpp
dp[0][capacity]
=
(capacity / wt[0]) * val[0]
```

---

## Example

```txt
wt[0] = 3
val[0] = 10

capacity = 8
```

Can take:

```txt
2 items
```

Profit:

```txt
20
```

---

# Transition

## Not Take

Ignore current item:

```cpp
dp[i-1][capacity]
```

---

## Take

Take current item once:

```cpp
val[i]
+
dp[i][capacity - wt[i]]
```

Notice:

```cpp
dp[i]
```

not:

```cpp
dp[i-1]
```

because:

```txt
Current item can be reused
```

---

## Final Transition

```cpp
dp[i][capacity]
=
max(
    dp[i-1][capacity],
    val[i]
    +
    dp[i][capacity-wt[i]]
)
```

---

# Why It Works

Every optimal solution:

```txt
Either uses current item
or does not use current item
```

If it uses current item:

```txt
Take one copy
+
Solve remaining capacity
```

Since reuse is allowed:

```txt
Current item remains available
```

Hence:

```cpp
dp[i][capacity-wt[i]]
```

is correct.

---

# Space Optimization

Observation:

```txt
Current row depends on:

dp[i-1][capacity]

and

dp[i][capacity-wt[i]]
```

Only one row is required.

---

# 1D State

```cpp
dp[capacity]
```

Meaning:

```txt
Maximum profit obtainable
for this capacity
using processed items
```

---

# Initialization

Using only first item:

```cpp
for(capacity = 1; capacity <= W; capacity++)
    dp[capacity]
    =
    (capacity / wt[0]) * val[0];
```

---

# Transition

```cpp
dp[capacity]
=
max(
    dp[capacity],
    dp[capacity-wt[i]]
    +
    val[i]
)
```

---

# Iteration Order

```cpp
for(capacity = wt[i];
    capacity <= W;
    capacity++)
```

Forward iteration.

---

# Why Forward Iteration?

Need:

```cpp
dp[capacity-wt[i]]
```

to already contain solutions using:

```txt
Current item
```

multiple times.

Example:

```txt
wt = 3

capacity = 9
```

Need:

```txt
9
↓
6
↓
3
↓
0
```

which corresponds to:

```txt
3 + 3 + 3
```

Therefore:

```txt
Forward Iteration ✓
```

---

# Why Backward Iteration Fails?

Backward iteration prevents:

```txt
Using current item again
```

and converts the problem into:

```txt
0/1 Knapsack
```

which is wrong.

---

# Complexity

## 2D DP

Time:

```txt
O(n × W)
```

Space:

```txt
O(n × W)
```

---

## 1D DP

Time:

```txt
O(n × W)
```

Space:

```txt
O(W)
```

---

# Common Mistakes

### ❌ Using 0/1 Knapsack Transition

Wrong:

```cpp
dp[i-1][capacity-wt[i]]
```

This allows taking current item only once.

---

### ❌ Backward Iteration

Wrong:

```cpp
for(capacity = W;
    capacity >= wt[i];
    capacity--)
```

This converts the problem into:

```txt
0/1 Knapsack
```

---

### ❌ Incorrect Base Case

Wrong:

```cpp
dp[0][capacity] = val[0]
```

Need:

```cpp
(capacity/wt[0]) * val[0]
```

because the item can be reused.

---

# Pattern Recognition

Whenever you see:

```txt
Maximum profit

Unlimited reuse

Weight + Value

Capacity constraint
```

Think:

```txt
Unbounded Knapsack
```

---

# Related Problems

### Same Pattern

[https://leetcode.com/problems/coin-change/](https://leetcode.com/problems/coin-change/)

[https://leetcode.com/problems/coin-change-ii/](https://leetcode.com/problems/coin-change-ii/)

[https://www.geeksforgeeks.org/problems/rod-cutting0840/1](https://www.geeksforgeeks.org/problems/rod-cutting0840/1)

---

### Compare With

[https://www.geeksforgeeks.org/problems/0-1-knapsack-problem0945/1](https://www.geeksforgeeks.org/problems/0-1-knapsack-problem0945/1)

Difference:

```txt
0/1 Knapsack
→ dp[i-1][W-wt]
→ Backward Iteration

Unbounded Knapsack
→ dp[i][W-wt]
→ Forward Iteration
```

---

# Interview Trigger

```txt
If you see:

Unlimited reuse

Maximize value

Weight and profit

Capacity constraint

Think:

Unbounded Knapsack

Transition:

dp[i][W]
=
max(
    dp[i-1][W],
    val[i] + dp[i][W-wt[i]]
)

1D DP:
Forward Iteration
```
