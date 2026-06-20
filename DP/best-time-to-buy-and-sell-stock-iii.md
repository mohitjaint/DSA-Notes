# Best Time to Buy and Sell Stock III

## References

Question:

[https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)

Related Problems:

- Best Time to Buy and Sell Stock II
- Best Time to Buy and Sell Stock IV
- Best Time to Buy and Sell Stock with Cooldown
- Best Time to Buy and Sell Stock with Transaction Fee

---

# Pattern

```text
DP on States

Stock DP

3D DP
```

---

# Problem Statement

You may perform

```text
At most 2 transactions
```

A transaction consists of

```text
Buy

↓

Sell
```

At any time,

```text
Only one stock can be held.
```

Find the maximum profit.

---

# Key Observation

Compared to Stock II,

```text
Unlimited Transactions
```

becomes

```text
Maximum 2 Transactions
```

Therefore, the previous state

```text
(day, buy)
```

is no longer sufficient.

We must also know

```text
How many transactions are still available.
```

---

# DP State

```cpp
dp[i][buy][cap]
```

Meaning:

```text
Maximum profit

starting from day i

buy

↓

1 → Allowed to Buy

0 → Holding a Stock

cap

↓

Transactions Remaining
```

---

# Why Decrement on Sell?

A common mistake is decreasing the transaction count while buying.

Wrong:

```text
Buy

↓

Transactions--
```

Correct:

```text
Buy

↓

Still holding stock

↓

Sell

↓

Transaction Completed

↓

Transactions--
```

Therefore

---

### Buy

```cpp
cap remains same
```

---

### Sell

```cpp
cap--
```

---

# Base Cases

## No Days Left

```cpp
if(i == n)
    return 0;
```

---

## No Transactions Left

```cpp
if(cap == 0)
    return 0;
```

No further profit can be earned.

---

# Transition

## Buy State

Two choices.

### Buy

```cpp
-prices[i]

+

solve(i+1,0,cap)
```

---

### Skip

```cpp
solve(i+1,1,cap)
```

Transition

```cpp
max(
    -prices[i]+solve(i+1,0,cap),
    solve(i+1,1,cap)
)
```

---

## Sell State

Two choices.

### Sell

```cpp
prices[i]

+

solve(i+1,1,cap-1)
```

---

### Hold

```cpp
solve(i+1,0,cap)
```

Transition

```cpp
max(
    prices[i]+solve(i+1,1,cap-1),
    solve(i+1,0,cap)
)
```

---

# Memoization

State

```cpp
dp[i][buy][cap]
```

Dimensions

```text
n

×

2

×

3
```

Why 3?

Because

```text
cap

=

0

1

2
```

---

# Tabulation

State remains identical.

```cpp
dp[i][buy][cap]
```

Fill

```text
Bottom

↓

Top
```

because every state depends upon

```text
Day + 1
```

---

# Base Initialization

```cpp
dp[n][buy][cap] = 0

for every buy

for every cap
```

Also

```cpp
dp[i][buy][0] = 0
```

for every day.

---

# Transition

## Buy

```cpp
dp[i][1][cap]

=

max(

-prices[i]

+

dp[i+1][0][cap],

dp[i+1][1][cap]

)
```

---

## Sell

```cpp
dp[i][0][cap]

=

max(

prices[i]

+

dp[i+1][1][cap-1],

dp[i+1][0][cap]

)
```

Answer

```cpp
dp[0][1][2]
```

---

# Why Return

```cpp
dp[0][1][2]
```

Initially

```text
Day = 0

No stock owned

2 transactions remaining
```

Exactly matches

```text
buy = 1

cap = 2
```

---

# Space Optimization

Observation

Each state depends only on

```text
Next Day
```

Therefore,

only two layers are required.

---

# State Mapping

```cpp
ahead[buy][cap]
```

means

```text
dp[i+1][buy][cap]
```

---

```cpp
curr[buy][cap]
```

means

```text
dp[i][buy][cap]
```

---

# Transition

```cpp
curr[1][cap]=max(
    -prices[i]+ahead[0][cap],
    ahead[1][cap]
);

curr[0][cap]=max(
    prices[i]+ahead[1][cap-1],
    ahead[0][cap]
);
```

After finishing the entire row

```cpp
ahead=curr;
```

---

# Common Mistake #1

Decreasing transactions while buying.

Wrong

```cpp
Buy

↓

cap--
```

Correct

```cpp
Sell

↓

cap--
```

---

# Common Mistake #2

Returning

```cpp
dp[0][0][2]
```

Wrong.

Initially

```text
No stock is owned.
```

Return

```cpp
dp[0][1][2]
```

---

# Common Mistake #3

Updating

```cpp
ahead = curr;
```

inside the `cap` loop.

Wrong

```cpp
for(cap){

...

ahead=curr;
}
```

This mixes

```text
Current Day

and

Next Day
```

Always compute the **entire row** first.

Then

```cpp
ahead=curr;
```

---

# Common Mistake #4

Decrementing the wrong loop direction.

Example:

```cpp
for(int i=n-1;i>=0;i++)
```

instead of

```cpp
for(int i=n-1;i>=0;i--)
```

This causes

```text
Heap Buffer Overflow
```

because `i` keeps increasing, leading to out-of-bounds accesses like:

```cpp
prices[i]
```

or

```cpp
dp[i+1]
```

When AddressSanitizer reports a buffer overflow, always verify loop directions (`i++` vs `i--`) before suspecting the DP recurrence.

---

# Complexity

## Memoization

Time

```text
O(n × 2 × 3)

=

O(n)
```

Space

```text
O(n × 2 × 3)

=

O(n)
```

---

## Tabulation

Time

```text
O(n)
```

Space

```text
O(n)
```

---

## Space Optimized

Time

```text
O(n)
```

Space

```text
O(2 × 3)

=

O(1)
```

---

# Relation to Stock II

| Stock II               | Stock III              |
| ---------------------- | ---------------------- |
| `(day, buy)`           | `(day, buy, cap)`      |
| Unlimited Transactions | Maximum 2 Transactions |
| Buy/Sell transitions   | Same                   |
| One extra state        | Transactions Remaining |

---

# Pattern Recognition

Whenever you see

```text
At most K transactions
```

Think

```text
State

↓

(day,

buy,

transactionsRemaining)
```

Buy

↓

```text
Buy

or

Skip
```

Sell

↓

```text
Sell

or

Hold
```

Only

```text
Sell
```

reduces the transaction count.

---

# Interview Trigger

```text
If you see

Stock

+

Limited Transactions

Think

State

↓

(day,

buy,

transactionsRemaining)

Buy

↓

Buy

or

Skip

Sell

↓

Sell

or

Hold

↓

transactionsRemaining--

Tabulation

↓

Next Day Dependency

↓

2 Layers

↓

Space O(1)
```

---

# Stock DP Progression

| Problem         | State                               |
| --------------- | ----------------------------------- |
| Stock I         | Greedy (Prefix Minimum)             |
| Stock II        | `(day, buy)`                        |
| Stock III       | `(day, buy, transactionsRemaining)` |
| Stock IV        | `(day, buy, k)`                     |
| Cooldown        | `(day, buy)` (modified transition)  |
| Transaction Fee | `(day, buy)` (modified transition)  |

## Core Insight

Across all stock DP problems, the **state machine never changes**:

```text
Buy State

↓

Buy

OR

Skip

Sell State

↓

Sell

OR

Hold
```

The only thing that changes is the **state space** (transactions, cooldown, fee, etc.). Once you master this framework, every stock DP problem becomes a variation of the same underlying DP rather than a completely new problem.
