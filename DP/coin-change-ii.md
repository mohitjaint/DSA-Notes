# 518. Coin Change II

## References

Question:

[https://leetcode.com/problems/coin-change-ii/](https://leetcode.com/problems/coin-change-ii/)

Related Problems:

[https://leetcode.com/problems/coin-change/](https://leetcode.com/problems/coin-change/)

[https://www.geeksforgeeks.org/problems/minimum-number-of-coins4426/1](https://www.geeksforgeeks.org/problems/minimum-number-of-coins4426/1)

[https://leetcode.com/problems/perfect-squares/](https://leetcode.com/problems/perfect-squares/)

[https://www.geeksforgeeks.org/problems/perfect-sum-problem5633/1](https://www.geeksforgeeks.org/problems/perfect-sum-problem5633/1)

---

# Pattern

```txt
Unbounded Knapsack

Count Number of Ways

DP on Amount
```

---

# Key Observation

We need:

```txt
Number of ways
to form amount
```

using:

```txt
Unlimited copies
of each coin
```

This is the signature of:

```txt
Unbounded Knapsack Counting DP
```

---

# Constraint Analysis

```txt
amount ≤ 5000
coins.length ≤ 300
```

Notice:

```txt
Amount is manageable
```

Therefore:

```txt
DP on Amount ✓
```

---

# Why Greedy Fails

Example:

```txt
coins = [1,2,5]

amount = 5
```

Valid combinations:

```txt
5

2+2+1

2+1+1+1

1+1+1+1+1
```

Need:

```txt
Count all ways
```

Greedy cannot count combinations.

---

# Bruteforce

For every coin:

```txt
Take 0 times
Take 1 time
Take 2 times
...
```

Recurrence:

```txt
Try all possible counts
of every coin
```

Complexity:

```txt
Exponential
```

Too large.

---

# State

```cpp
dp[i][amt]
```

Meaning:

```txt
Number of ways
to form amount = amt

using coins[0...i]
```

---

# Base Case

Amount:

```cpp
amt = 0
```

can always be formed by:

```txt
Choosing nothing
```

Therefore:

```cpp
dp[i][0] = 1
```

for all:

```cpp
i
```

---

For first coin only:

```cpp
coins[0]
```

If:

```cpp
amt % coins[0] == 0
```

Then:

```txt
Exactly one way
```

Example:

```txt
coins[0] = 2

4 = 2+2
6 = 2+2+2
```

Therefore:

```cpp
dp[0][amt] = 1
```

Otherwise:

```cpp
dp[0][amt] = 0
```

---

# Transition

## Not Take

Ignore current coin:

```cpp
dp[i-1][amt]
```

---

## Take

Use current coin once:

```cpp
dp[i][amt-coins[i]]
```

Notice:

```cpp
dp[i]
```

not:

```cpp
dp[i-1]
```

because the coin can be reused.

---

## Final Transition

```cpp
dp[i][amt]
=
dp[i-1][amt]
+
dp[i][amt-coins[i]]
```

---

# Why It Works

Every valid combination:

```txt
Either uses current coin
Or does not use current coin
```

These cases are disjoint.

Therefore:

```txt
Total Ways

=
Not Take Ways
+
Take Ways
```

---

# Space Optimization

Observation:

```txt
Current row depends on:

dp[i-1][amt]

and

dp[i][amt-coins[i]]
```

Therefore:

```txt
1D DP Possible
```

---

# 1D State

```cpp
dp[amt]
```

Meaning:

```txt
Number of ways
to form amt
using processed coins
```

---

# Initialization

```cpp
dp[0] = 1
```

Reason:

```txt
One way to make amount 0

Choose nothing
```

---

# Transition

```cpp
dp[amt]
+=
dp[amt-coins[i]]
```

---

# Most Important Observation

## Forward Iteration Required

```cpp
for(int amt = coin;
    amt <= amount;
    amt++)
```

---

## Why?

Need:

```cpp
dp[amt-coins[i]]
```

to already include ways using:

```cpp
coins[i]
```

multiple times.

Example:

```txt
coin = 2

amount = 6
```

Need:

```txt
6
↓
4
↓
2
↓
0
```

which represents:

```txt
2 + 2 + 2
```

Therefore:

```txt
Forward Iteration ✓
```

---

## Why Backward Iteration Fails?

Backward iteration prevents:

```txt
Reusing current coin
```

and converts the problem into:

```txt
0/1 Knapsack Counting
```

which is wrong.

---

# Complexity

## 2D DP

Time:

```txt
O(n × amount)
```

Space:

```txt
O(n × amount)
```

---

## 1D DP

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

### ❌ Using Backward Iteration

Wrong:

```cpp
for(amt = amount;
    amt >= coin;
    amt--)
```

This becomes:

```txt
0/1 Knapsack
```

---

### ❌ Using

```cpp
dp[i-1][amt-coins[i]]
```

Wrong.

This prevents reusing the current coin.

---

### ❌ Counting Permutations Instead of Combinations

Wrong loop order:

```cpp
for(amount)
    for(coins)
```

Counts:

```txt
1+2

and

2+1
```

separately.

Problem asks:

```txt
Combinations
```

not permutations.

---

### ❌ Confusing With Coin Change I

Coin Change I:

```txt
Minimum Coins
```

Uses:

```cpp
min(...)
```

Coin Change II:

```txt
Count Ways
```

Uses:

```cpp
+
```

---

# Pattern Recognition

Whenever you see:

```txt
Count ways

Unlimited reuse

Coin denominations

Target amount
```

Think:

```txt
Unbounded Knapsack Counting
```

---

# Related Problems

### Same Pattern

[https://leetcode.com/problems/coin-change/](https://leetcode.com/problems/coin-change/)

[https://leetcode.com/problems/perfect-squares/](https://leetcode.com/problems/perfect-squares/)

[https://www.geeksforgeeks.org/problems/minimum-number-of-coins4426/1](https://www.geeksforgeeks.org/problems/minimum-number-of-coins4426/1)

---

### Compare With

[https://www.geeksforgeeks.org/problems/perfect-sum-problem5633/1](https://www.geeksforgeeks.org/problems/perfect-sum-problem5633/1)

Difference:

```txt
Perfect Sum
→ 0/1 Knapsack
→ Backward Iteration

Coin Change II
→ Unbounded Knapsack
→ Forward Iteration
```

---

# Interview Trigger

```txt
If you see:

Count ways

Unlimited usage allowed

Target amount

Think:

Unbounded Knapsack

Transition:

dp[i][amt]
=
dp[i-1][amt]
+
dp[i][amt-coin]

1D DP:
Forward Iteration
```
