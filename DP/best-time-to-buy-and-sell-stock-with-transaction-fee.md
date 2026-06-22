# Best Time to Buy and Sell Stock with Transaction Fee

## References

Question:

[https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)

Related Problems:

- Best Time to Buy and Sell Stock II
- Best Time to Buy and Sell Stock III
- Best Time to Buy and Sell Stock IV
- Best Time to Buy and Sell Stock with Cooldown

---

# Pattern

```text
DP on States

Stock DP

Transaction Cost DP
```

---

# Problem Statement

You may perform

```text
Unlimited Transactions
```

Every completed transaction incurs a

```text
Fixed Transaction Fee
```

Find the maximum possible profit.

---

# Key Observation

Compared to **Stock II**

```text
Buy

↓

Sell

↓

Profit
```

becomes

```text
Buy

↓

Sell

↓

Pay Fee

↓

Profit
```

The state does **not** change.

Only the **Sell transition** changes.

---

# DP State

```cpp
dp[day][buy]
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

Currently Holding a Stock
```

---

# Base Case

```cpp
if(day == n)
    return 0;
```

No future profit is possible.

---

# Transition

## Buy State

Choices

### Buy

```cpp
-prices[day]

+

solve(day+1,0)
```

---

### Skip

```cpp
solve(day+1,1)
```

Transition

```cpp
max(
    -prices[day] + solve(day+1,0),
    solve(day+1,1)
)
```

---

## Sell State

Choices

### Sell

Selling earns

```text
Selling Price

-

Transaction Fee
```

Transition

```cpp
prices[day]

-

fee

+

solve(day+1,1)
```

---

### Hold

```cpp
solve(day+1,0)
```

Transition

```cpp
max(
    prices[day] - fee + solve(day+1,1),
    solve(day+1,0)
)
```

---

# Why Subtract Fee While Selling?

One transaction is

```text
Buy

↓

Sell
```

The transaction completes when selling.

Therefore charging the fee while selling is natural.

Example

```text
Buy = 3

Sell = 10

Fee = 2
```

Profit

```text
10

-

3

-

2

=

5
```

---

# Can Fee Be Charged During Buying?

Yes.

Instead of

```cpp
prices[i] - fee + aheadBuy
```

you may write

```cpp
-(prices[i] + fee)
```

during buying.

Both approaches are mathematically equivalent.

Most editorials subtract the fee while selling because a transaction finishes there.

---

# Memoization

State

```cpp
dp[day][buy]
```

Dimensions

```text
n

×

2
```

---

# Tabulation

Fill

```text
Last Day

↓

First Day
```

because every state depends only on

```text
Next Day
```

---

# Base Initialization

```cpp
dp[n][0] = 0;

dp[n][1] = 0;
```

---

# Transition

## Buy

```cpp
dp[day][1]

=

max(

-prices[day]

+

dp[day+1][0],

dp[day+1][1]

)
```

---

## Sell

```cpp
dp[day][0]

=

max(

prices[day]

-

fee

+

dp[day+1][1],

dp[day+1][0]

)
```

Answer

```cpp
dp[0][1]
```

---

# Space Optimization

Only

```text
day+1
```

is required.

Maintain

```cpp
aheadBuy

aheadSell

currBuy

currSell
```

---

# State Mapping

| Variable    | Represents     |
| ----------- | -------------- |
| `currBuy`   | `dp[day][1]`   |
| `currSell`  | `dp[day][0]`   |
| `aheadBuy`  | `dp[day+1][1]` |
| `aheadSell` | `dp[day+1][0]` |

---

# Transition

## Buy

```cpp
currBuy = max(
    -prices[day] + aheadSell,
    aheadBuy
);
```

---

## Sell

```cpp
currSell = max(
    prices[day] - fee + aheadBuy,
    aheadSell
);
```

---

# Update

```cpp
aheadBuy = currBuy;
aheadSell = currSell;
```

Answer

```cpp
return aheadBuy;
```

---

# Difference from Stock II

Only one line changes.

Stock II

```cpp
currSell = max(
    prices[day] + aheadBuy,
    aheadSell
);
```

Transaction Fee

```cpp
currSell = max(
    prices[day] - fee + aheadBuy,
    aheadSell
);
```

Everything else remains identical.

---

# Common Mistake #1

Subtracting fee during both Buy and Sell.

Wrong

```text
Buy

↓

Fee

Sell

↓

Fee
```

Fee should be charged exactly once per transaction.

---

# Common Mistake #2

Updating

```cpp
ahead = curr;
```

before computing both states.

Always compute

```text
Buy

Sell
```

first.

Then update the next-day states.

---

# Common Mistake #3

Returning

```cpp
dp[0][0]
```

Wrong.

Initially

```text
No stock is owned.
```

Return

```cpp
dp[0][1]
```

---

# Complexity

## Memoization

Time

```text
O(n)
```

Space

```text
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
O(1)
```

---

# Relation with Previous Stock Problems

| Problem         | State               | Modification                  |
| --------------- | ------------------- | ----------------------------- |
| Stock II        | `(day, buy)`        | Unlimited transactions        |
| Stock III       | `(day, buy, cap=2)` | Add transaction limit         |
| Stock IV        | `(day, buy, cap=K)` | Generalized transaction limit |
| Cooldown        | `(day, buy)`        | Sell jumps to `day+2`         |
| Transaction Fee | `(day, buy)`        | Subtract fee during sell      |

---

# Pattern Recognition

Whenever you see

```text
Each transaction costs

Fee
```

Think

```text
State

↓

(day,

buy)

↓

Sell Transition

↓

Subtract Fee
```

No new DP state is required.

---

# Interview Trigger

```text
Stock Problem

+

Transaction Fee

↓

State

(day,

buy)

↓

Buy

↓

Buy / Skip

↓

Sell

↓

Sell / Hold

↓

Selling Profit

=

Selling Price

-

Fee

↓

Next Day Dependency

↓

4 Variables

↓

O(1) Space
```

---

# Stock DP Summary

| Problem         | State               | Key Change                   |
| --------------- | ------------------- | ---------------------------- |
| Stock I         | Greedy              | One transaction              |
| Stock II        | `(day, buy)`        | Unlimited transactions       |
| Stock III       | `(day, buy, cap=2)` | Add transaction count        |
| Stock IV        | `(day, buy, cap=K)` | Generalize transaction count |
| Cooldown        | `(day, buy)`        | Sell moves to `day+2`        |
| Transaction Fee | `(day, buy)`        | Deduct fee during selling    |

---

# Core Insight

The entire Stock DP series is built on the **same two-state machine**:

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

Each variation modifies only one aspect:

- **Stock III / IV:** Add a **transaction dimension**.
- **Cooldown:** Change the sell dependency from `day+1` to `day+2`.
- **Transaction Fee:** Deduct the fee in the sell transition.

The DP framework itself remains unchanged, making these problems natural extensions of the same underlying state machine rather than completely different algorithms.
