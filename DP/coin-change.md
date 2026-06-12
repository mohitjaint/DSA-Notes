# 322. Coin Change

## References

Question:

[https://leetcode.com/problems/coin-change/](https://leetcode.com/problems/coin-change/)

Related Problems:

[https://leetcode.com/problems/coin-change-ii/](https://leetcode.com/problems/coin-change-ii/)

[https://www.geeksforgeeks.org/problems/minimum-number-of-coins4426/1](https://www.geeksforgeeks.org/problems/minimum-number-of-coins4426/1)

[https://leetcode.com/problems/perfect-squares/](https://leetcode.com/problems/perfect-squares/)

[https://leetcode.com/problems/combination-sum-iv/](https://leetcode.com/problems/combination-sum-iv/)

---

# Pattern

```txt
Unbounded Knapsack

Minimum Number of Items

DP on Target Sum
```

---

# Key Observation

We need:

```txt
Minimum number of coins
to make amount
```

Each coin can be used:

```txt
Unlimited times
```

This is the signature of:

```txt
Unbounded Knapsack
```

---

# Constraint Analysis

```txt
coins.length ≤ 12

amount ≤ 10^4
```

Notice:

```txt
Number of coin types is small
Amount is also manageable
```

Therefore:

```txt
DP on amount ✓
```

---

# Why Greedy Fails

Greedy:

```txt
Always take largest coin first
```

does not always give the minimum number of coins.

Example:

```txt
coins = [1,3,4]

amount = 6
```

Greedy:

```txt
4 + 1 + 1
```

Answer:

```txt
3 coins
```

Optimal:

```txt
3 + 3
```

Answer:

```txt
2 coins
```

Therefore:

```txt
Greedy ✗
DP ✓
```

---

# Bruteforce

For every coin:

```txt
Take
Take Again
Take Again
...
Not Take
```

Recurrence:

```txt
Try all possible counts
of every coin.
```

Complexity:

```txt
Exponential
```

Too large.

---

# State

```cpp
dp[i][target]
```

Meaning:

```txt
Minimum coins required
to form target
using coins [0...i].
```

---

# Base Case

Using only:

```cpp
coins[0]
```

If:

```cpp
target % coins[0] == 0
```

Then:

```cpp
dp[0][target]
=
target / coins[0]
```

Otherwise:

```cpp
Impossible
```

Store:

```cpp
1e7
```

as infinity.

---

# Transition

## Not Take

Ignore current coin:

```cpp
dp[i-1][target]
```

---

## Take

Use one current coin:

```cpp
1 + dp[i][target - coins[i]]
```

Notice:

```cpp
dp[i]
```

not:

```cpp
dp[i-1]
```

because we can reuse the same coin.

---

## Final Transition

```cpp
dp[i][target]
=
min(
    dp[i-1][target],
    1 + dp[i][target-coins[i]]
)
```

---

# Why It Works

Every optimal solution:

```txt
Either uses current coin
or does not use it.
```

If it uses the coin:

```txt
Take one coin
+
Solve remaining amount
```

Since coins are unlimited:

```txt
Current coin remains available.
```

Therefore:

```cpp
dp[i][target-coins[i]]
```

is correct.

---

# Space Optimization

Observation:

```txt
Current row depends on:

dp[i-1][target]

and

dp[i][target-coins[i]]
```

Only one row is required.

---

# 1D DP

## State

```cpp
dp[target]
```

Meaning:

```txt
Minimum coins needed
to form target.
```

---

## Initialization

```cpp
dp[0] = 0
```

All other values:

```cpp
INF
```

---

## Transition

```cpp
dp[target]
=
min(
    dp[target],
    1 + dp[target-coin]
)
```

---

# Iteration Order

```cpp
for(target = coin;
    target <= amount;
    target++)
```

Forward iteration.

---

# Why Forward Iteration?

We want:

```cpp
dp[target-coin]
```

to already contain solutions using:

```cpp
coin
```

multiple times.

Example:

```txt
amount = 9
coin = 3
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

Backward iteration would prevent:

```txt
Reusing the same coin
```

and effectively convert the problem into:

```txt
0/1 Knapsack
```

which is wrong.

---

# Complexity

## Tabulation

Time:

```txt
O(n × amount)
```

Space:

```txt
O(n × amount)
```

---

## Space Optimized

Time:

```txt
O(n × amount)
```

Space:

```txt
O(amount)
```

---

# Common Mistakes

### ❌ Using Greedy

Greedy is not guaranteed to be optimal.

---

### ❌ Using

```cpp
dp[i-1][target-coins[i]]
```

This converts the problem into:

```txt
0/1 Knapsack
```

Current coin cannot be reused.

Wrong.

---

### ❌ Backward Iteration in 1D DP

Wrong:

```cpp
for(target = amount;
    target >= coin;
    target--)
```

This prevents multiple usage of the same coin.

---

### ❌ Forget Impossible States

Use:

```cpp
INF = 1e7
```

or

```cpp
INT_MAX/2
```

to represent:

```txt
Impossible
```

states.

---

### ❌ Returning INF

If:

```cpp
dp[amount]
```

is still infinity:

```cpp
return -1;
```

---

# Pattern Recognition

Whenever you see:

```txt
Minimum number of coins

Minimum number of cuts

Minimum number of items

Unlimited reuse allowed
```

Think:

```txt
Unbounded Knapsack
```

---

# Related Problems

### Same Pattern

[https://leetcode.com/problems/coin-change-ii/](https://leetcode.com/problems/coin-change-ii/)

[https://www.geeksforgeeks.org/problems/minimum-number-of-coins4426/1](https://www.geeksforgeeks.org/problems/minimum-number-of-coins4426/1)

[https://leetcode.com/problems/perfect-squares/](https://leetcode.com/problems/perfect-squares/)

---

### Compare With

[https://www.geeksforgeeks.org/problems/subset-sum-problem-1611555638/1](https://www.geeksforgeeks.org/problems/subset-sum-problem-1611555638/1)

[https://www.geeksforgeeks.org/problems/perfect-sum-problem5633/1](https://www.geeksforgeeks.org/problems/perfect-sum-problem5633/1)

Difference:

```txt
Subset Sum Family
→ 0/1 Knapsack
→ Backward Iteration

Coin Change Family
→ Unbounded Knapsack
→ Forward Iteration
```

---

# Interview Trigger

```txt
If you see:

Minimum items

Unlimited reuse

Target Sum

Coin denominations

Think:

Unbounded Knapsack

Take:
dp[i][target-coin]

1D DP:
Forward Iteration
```
