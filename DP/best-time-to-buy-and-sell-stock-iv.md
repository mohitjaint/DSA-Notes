# Best Time to Buy and Sell Stock IV

## References

Question:

[https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/)

Related Problems:

- Best Time to Buy and Sell Stock II
- Best Time to Buy and Sell Stock III
- Best Time to Buy and Sell Stock with Cooldown
- Best Time to Buy and Sell Stock with Transaction Fee

---

# Pattern

```text
DP on States

Stock DP

3D DP

Generalization of Stock III
```

---

# Problem Statement

You may perform

```text
At most K transactions
```

One transaction consists of

```text
Buy

↓

Sell
```

At any time,

```text
Only one stock can be held.
```

Find the maximum possible profit.

---

# Key Observation

This problem is simply the generalized version of Stock III.

Stock III

```text
Transactions = 2
```

Stock IV

```text
Transactions = K
```

The DP recurrence remains exactly the same.

Only the range of the transaction state changes.

---

# DP State

```cpp
dp[day][buy][transactionsRemaining]
```

Meaning

```text
Maximum profit

starting from current day

buy = 1

↓

Allowed to Buy

buy = 0

↓

Currently holding a stock

transactionsRemaining

↓

Number of complete transactions still available
```

---

# State Variables

```text
day

↓

Current day

buy

↓

1 = Buy State

0 = Sell State

transactionsRemaining

↓

Remaining transactions
```

---

# Base Cases

## No Days Left

```cpp
if(day == n)
    return 0;
```

---

## No Transactions Left

```cpp
if(transactionsRemaining == 0)
    return 0;
```

No further profit can be earned.

---

# Why Decrement Only While Selling?

A transaction is

```text
Buy

↓

Sell
```

Buying only starts a transaction.

Selling completes it.

Therefore

---

Buy

```text
Transactions remain unchanged
```

---

Sell

```text
TransactionsRemaining--
```

---

# Transition

## Buy State

Choices

### Buy

```cpp
-prices[day]

+

solve(day+1,0,transactionsRemaining)
```

---

### Skip

```cpp
solve(day+1,1,transactionsRemaining)
```

Transition

```cpp
max(
    -prices[day] + solve(day+1,0,transactionsRemaining),
    solve(day+1,1,transactionsRemaining)
)
```

---

## Sell State

Choices

### Sell

```cpp
prices[day]

+

solve(day+1,1,transactionsRemaining-1)
```

---

### Hold

```cpp
solve(day+1,0,transactionsRemaining)
```

Transition

```cpp
max(
    prices[day] + solve(day+1,1,transactionsRemaining-1),
    solve(day+1,0,transactionsRemaining)
)
```

---

# Memoization

State

```cpp
dp[day][buy][transactionsRemaining]
```

Dimensions

```text
n

×

2

×

(K+1)
```

---

# Tabulation

Fill from

```text
Last Day

↓

First Day
```

because every state depends on

```text
Next Day
```

---

# Base Initialization

At the last day

```cpp
dp[n][buy][transactionsRemaining]=0
```

for every

```text
buy

transactionsRemaining
```

Also

```cpp
dp[day][buy][0]=0
```

because

```text
No transactions left

↓

No profit possible
```

---

# Transition

## Buy

```cpp
dp[day][1][cap]

=

max(

-prices[day]

+

dp[day+1][0][cap],

dp[day+1][1][cap]

)
```

---

## Sell

```cpp
dp[day][0][cap]

=

max(

prices[day]

+

dp[day+1][1][cap-1],

dp[day+1][0][cap]

)
```

---

Answer

```cpp
dp[0][1][K]
```

---

# Space Optimization

Observation

Every state depends only on

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

represents

```text
dp[day+1][buy][cap]
```

---

```cpp
curr[buy][cap]
```

represents

```text
dp[day][buy][cap]
```

---

Transition

```cpp
curr[1][cap] = max(
    -prices[day] + ahead[0][cap],
    ahead[1][cap]
);

curr[0][cap] = max(
    prices[day] + ahead[1][cap-1],
    ahead[0][cap]
);
```

After computing the entire day

```cpp
ahead = curr;
```

---

# Important Optimization

Observe

```text
Maximum possible transactions

=

⌊n/2⌋
```

Reason

One transaction requires

```text
Buy

↓

Sell
```

So every completed transaction consumes at least two trading opportunities.

If

```cpp
K >= n/2
```

then

```text
Transaction limit is never reached.
```

The problem becomes

```text
Unlimited Transactions
```

which is exactly **Stock II**.

---

# Greedy Shortcut

When

```cpp
K >= n/2
```

use

```cpp
profit = 0;

for(int i=1;i<n;i++){

    if(prices[i]>prices[i-1])
        profit += prices[i]-prices[i-1];
}
```

Time

```text
O(n)
```

instead of

```text
O(nK)
```

---

# Why Does This Optimization Work?

Suppose

```text
n = 6
```

Maximum transactions possible

```text
Buy Sell

Buy Sell

Buy Sell
```

Only

```text
3
```

transactions are possible.

Even if

```text
K = 100
```

you can never perform more than

```text
3
```

transactions.

Therefore,

```text
K behaves like Infinity
```

and the greedy solution from Stock II becomes optimal.

---

# Common Mistake #1

Reducing transaction count while buying.

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

Updating

```cpp
ahead = curr;
```

inside the transaction loop.

Wrong

```cpp
for(cap){

...

ahead=curr;
}
```

Always compute the entire current day first.

Then

```cpp
ahead=curr;
```

---

# Common Mistake #3

Returning

```cpp
dp[0][0][K]
```

Wrong.

Initially

```text
No stock owned
```

Return

```cpp
dp[0][1][K]
```

---

# Common Mistake #4

Missing the Greedy Optimization

Without

```cpp
if(K>=n/2)
```

complexity remains

```text
O(nK)
```

even when

```text
K

>>

n
```

The greedy shortcut improves such cases to

```text
O(n)
```

---

# Complexity

## Memoization

Time

```text
O(nK)
```

Space

```text
O(nK)
```

---

## Tabulation

Time

```text
O(nK)
```

Space

```text
O(nK)
```

---

## Space Optimized

Time

```text
O(nK)
```

Space

```text
O(K)
```

---

## Greedy Shortcut

When

```text
K>=n/2
```

Time

```text
O(n)
```

Space

```text
O(1)
```

---

# Relation with Previous Problems

| Problem   | State               |
| --------- | ------------------- |
| Stock I   | Greedy              |
| Stock II  | `(day, buy)`        |
| Stock III | `(day, buy, cap=2)` |
| Stock IV  | `(day, buy, cap=K)` |

---

# Pattern Recognition

Whenever you see

```text
At most K transactions
```

think

```text
State

↓

(day,

buy,

transactionsRemaining)
```

The recurrence never changes.

Only the transaction limit changes.

---

# Interview Trigger

```text
Stock Problem

↓

Limited Transactions

↓

State

(day,

buy,

transactionsRemaining)

↓

Buy

↓

Buy / Skip

↓

Sell

↓

Sell / Hold

↓

transactionsRemaining--

↓

Next Day Dependency

↓

Two Layers

↓

O(K) Space
```

---

# Core Insight

Stock III and Stock IV are fundamentally the **same DP**.

Stock III is simply the special case:

```text
K = 2
```

Once you derive the recurrence for **K transactions**, Stock III becomes an immediate specialization, and the only additional optimization for Stock IV is recognizing that when:

```text
K ≥ n/2
```

the transaction limit is effectively infinite, allowing you to switch to the **Stock II greedy solution** in linear time.
