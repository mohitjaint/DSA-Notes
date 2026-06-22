# Best Time to Buy and Sell Stock with Cooldown

## References

Question:

[https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

Related Problems:

- Best Time to Buy and Sell Stock II
- Best Time to Buy and Sell Stock III
- Best Time to Buy and Sell Stock IV
- Best Time to Buy and Sell Stock with Transaction Fee

---

# Pattern

```text
DP on States

Stock DP

Cooldown DP
```

---

# Problem Statement

You may perform

```text
Unlimited Transactions
```

After selling a stock,

```text
You cannot buy on the next day.
```

There is exactly **one cooldown day** after every sell.

---

# Key Observation

Compared to **Stock II**

```text
Sell

↓

Buy Again Next Day
```

becomes

```text
Sell

↓

Cooldown

↓

Buy Again
```

The Buy transition remains unchanged.

Only the Sell transition changes.

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
if(day >= n)
    return 0;
```

No days left.

Notice

```text
day == n

or

day == n+1
```

both return zero because after the last day no further profit is possible.

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

After selling,

```text
Next day is cooldown.
```

Therefore

```cpp
prices[day]

+

solve(day+2,1)
```

---

### Hold

```cpp
solve(day+1,0)
```

Transition

```cpp
max(
    prices[day] + solve(day+2,1),
    solve(day+1,0)
)
```

---

# Difference from Stock II

Only one line changes.

Stock II

```cpp
prices[day] + solve(day+1,1)
```

Cooldown

```cpp
prices[day] + solve(day+2,1)
```

Everything else remains identical.

---

# Memoization

State

```cpp
dp[day][buy]
```

Dimensions

```text
(n+2)

×

2
```

`n+2` is convenient because the recurrence accesses

```text
day+2
```

---

# Tabulation

Fill

```text
Last Day

↓

First Day
```

because every state depends on

```text
Next Day

or

Next-to-Next Day
```

---

# Base Initialization

```cpp
dp[n][0]=0
dp[n][1]=0

dp[n+1][0]=0
dp[n+1][1]=0
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

+

dp[day+2][1],

dp[day+1][0]

)
```

Answer

```cpp
dp[0][1]
```

---

# Space Optimization

## Observation

Each state depends on

```text
day+1

and

day+2
```

Therefore

```cpp
ahead1
```

stores

```text
dp[day+1]
```

and

```cpp
ahead2
```

stores

```text
dp[day+2]
```

---

# State Mapping

| Variable     | Represents     |
| ------------ | -------------- |
| `currBuy`    | `dp[day][1]`   |
| `currSell`   | `dp[day][0]`   |
| `aheadBuy1`  | `dp[day+1][1]` |
| `aheadSell1` | `dp[day+1][0]` |
| `aheadBuy2`  | `dp[day+2][1]` |
| `aheadSell2` | `dp[day+2][0]` |

---

# Transition

## Buy

```cpp
currBuy = max(
    -prices[day] + aheadSell1,
    aheadBuy1
);
```

---

## Sell

```cpp
currSell = max(
    prices[day] + aheadBuy2,
    aheadSell1
);
```

---

# Update

After computing the entire day

```cpp
aheadBuy2 = aheadBuy1;
aheadSell2 = aheadSell1;

aheadBuy1 = currBuy;
aheadSell1 = currSell;
```

This shifts

```text
dp[day+1]

↓

dp[day+2]
```

before storing the current row as the new `day+1`.

---

# Why Two Future Rows?

Stock II

```text
Sell

↓

day+1
```

Cooldown

```text
Sell

↓

Cooldown

↓

day+2
```

Hence one additional future row is required.

---

# Common Mistake #1

Updating

```cpp
ahead1 = curr;
ahead2 = ahead1;
```

inside the Buy/Sell loop.

Wrong.

Compute

```text
Both Buy

and

Sell
```

first.

Only then update

```cpp
ahead2 = ahead1;
ahead1 = curr;
```

---

# Common Mistake #2

Using

```cpp
prices[day] + aheadBuy1
```

after selling.

Wrong.

Cooldown requires

```cpp
prices[day] + aheadBuy2
```

---

# Common Mistake #3

Thinking cooldown affects Buy.

Wrong.

Cooldown only affects

```text
Sell
```

The Buy transition remains identical to Stock II.

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

| Problem   | State             | Dependency       |
| --------- | ----------------- | ---------------- |
| Stock II  | `(day, buy)`      | `day+1`          |
| Stock III | `(day, buy, cap)` | `day+1`          |
| Stock IV  | `(day, buy, cap)` | `day+1`          |
| Cooldown  | `(day, buy)`      | `day+1`, `day+2` |

---

# Pattern Recognition

Whenever you see

```text
After selling,

wait X days
```

think

```text
Sell Transition

↓

Jump

X+1

days ahead
```

Example

Cooldown = 1

```text
Sell

↓

day+2
```

Cooldown = 2

```text
Sell

↓

day+3
```

---

# Interview Trigger

```text
Stock Problem

+

Cooldown

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

Sell

↓

Jump to day+2

↓

Needs

Two Future Rows

↓

Space O(1)
```

---

# Stock DP Progression

| Problem      | State               | Main Modification             |
| ------------ | ------------------- | ----------------------------- |
| Stock I      | Greedy              | One transaction               |
| Stock II     | `(day, buy)`        | Unlimited transactions        |
| Stock III    | `(day, buy, cap=2)` | Limited to 2 transactions     |
| Stock IV     | `(day, buy, cap=K)` | Generalized transaction limit |
| **Cooldown** | `(day, buy)`        | Sell jumps to `day+2`         |

---

# Core Insight

The **buy/sell state machine never changes** across the stock DP series:

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

The only modification in the Cooldown problem is the **sell transition**:

```text
Stock II

Sell

↓

day+1

Cooldown

Sell

↓

Cooldown

↓

day+2
```

This changes the dependency from **one future row** to **two future rows**, which is why the space-optimized solution requires maintaining `ahead1` (`day+1`) and `ahead2` (`day+2`).
