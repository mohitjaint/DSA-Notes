# Best Time to Buy and Sell Stock II

## References

Question:

[https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

Related Problems:

[https://leetcode.com/problems/best-time-to-buy-and-sell-stock/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

[https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)

[https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/)

[https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

[https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)

---

# Pattern

```txt
Greedy

OR

DP on States (Stock DP)
```

---

# Problem Statement

You may perform:

```txt
Unlimited Transactions
```

Restrictions:

```txt
Cannot hold more than one stock at a time.
```

Meaning:

```txt
Buy

↓

Sell

↓

Buy

↓

Sell
```

---

# Key Observation (Greedy)

Suppose prices are:

```txt
1 3 5
```

One transaction:

```txt
Buy 1

Sell 5

Profit = 4
```

Multiple transactions:

```txt
Buy 1

Sell 3

Profit = 2

Buy 3

Sell 5

Profit = 2
```

Total:

```txt
2 + 2 = 4
```

Exactly the same.

---

Therefore every increasing segment can be split into multiple smaller transactions.

So simply collect every positive increase.

---

# Greedy Algorithm

```cpp
int profit = 0;

for(int i = 1; i < prices.size(); i++){

    if(prices[i] > prices[i-1])
        profit += prices[i] - prices[i-1];
}

return profit;
```

Equivalent implementation:

```cpp
int mini = prices[0];
int profit = 0;

for(int i=1;i<n;i++){

    if(prices[i] > mini){

        profit += prices[i]-mini;

        mini = prices[i];
    }

    mini = min(mini, prices[i]);
}
```

---

# Why Greedy Works?

Whenever price increases:

```txt
A

↓

B
```

Profit:

```txt
B-A
```

Taking profit immediately never hurts because:

```txt
You can immediately buy again
on the same day.
```

Hence every positive increase should be taken.

---

# DP Approach

Although Greedy is optimal, this problem is usually solved using DP to introduce the Stock DP framework.

---

# DP State

```cpp
dp[i][buy]
```

Meaning:

```txt
Maximum profit

starting from day i

buy = 1

↓

Currently do NOT own a stock.

Allowed to Buy.

buy = 0

↓

Currently own a stock.

Allowed to Sell.
```

---

# Base Case

After the last day:

```cpp
dp[n][0] = 0;

dp[n][1] = 0;
```

No more transactions are possible.

---

# Transition

## Buy State

Two choices.

### Buy Today

```cpp
-price[i] + dp[i+1][0]
```

---

### Skip

```cpp
dp[i+1][1]
```

---

Transition:

```cpp
dp[i][1]
=
max(
    -price[i]+dp[i+1][0],
    dp[i+1][1]
)
```

---

## Sell State

Two choices.

### Sell Today

```cpp
price[i]+dp[i+1][1]
```

---

### Hold

```cpp
dp[i+1][0]
```

---

Transition

```cpp
dp[i][0]
=
max(
    price[i]+dp[i+1][1],
    dp[i+1][0]
)
```

---

# Complete Tabulation

```cpp
for(int i=n-1;i>=0;i--){

    dp[i][1]=max(
        -prices[i]+dp[i+1][0],
        dp[i+1][1]
    );

    dp[i][0]=max(
        prices[i]+dp[i+1][1],
        dp[i+1][0]
    );
}

return dp[0][1];
```

---

# Why Return `dp[0][1]`?

Initially:

```txt
No stock is owned.
```

Therefore initial state is:

```txt
Allowed to Buy
```

Hence answer:

```cpp
dp[0][1]
```

Not:

```cpp
max(dp[0][0],dp[0][1])
```

because

```txt
dp[0][0]

assumes

we already own a stock.
```

---

# Space Optimization (Two Arrays)

Observation:

Every state depends only on:

```txt
Next Day
```

Hence:

```cpp
ahead[2]

curr[2]
```

are sufficient.

---

# State Mapping

| Variable   | Represents   |
| ---------- | ------------ |
| `ahead[1]` | `dp[i+1][1]` |
| `ahead[0]` | `dp[i+1][0]` |
| `curr[1]`  | `dp[i][1]`   |
| `curr[0]`  | `dp[i][0]`   |

---

# Transition

```cpp
curr[1]=max(
    -prices[i]+ahead[0],
    ahead[1]
);

curr[0]=max(
    prices[i]+ahead[1],
    ahead[0]
);

ahead=curr;
```

---

# Four Variable Optimization

Only two states exist.

Vectors are unnecessary.

---

# State Mapping

| Variable    | Represents   |
| ----------- | ------------ |
| `aheadBuy`  | `dp[i+1][1]` |
| `aheadSell` | `dp[i+1][0]` |
| `currBuy`   | `dp[i][1]`   |
| `currSell`  | `dp[i][0]`   |

---

# Transition

```cpp
currBuy = max(
    -prices[i] + aheadSell,
    aheadBuy
);

currSell = max(
    prices[i] + aheadBuy,
    aheadSell
);

aheadBuy = currBuy;
aheadSell = currSell;
```

Answer:

```cpp
return aheadBuy;
```

---

# Common Mistake #1

Using

```cpp
+=
```

instead of

```cpp
=
```

Each DP state stores:

```txt
Maximum profit

from this state,

NOT cumulative profit.
```

---

# Common Mistake #2

Returning

```cpp
max(dp[0][0],dp[0][1])
```

Wrong.

Initial state is:

```txt
Buy
```

Return:

```cpp
dp[0][1]
```

---

# Common Mistake #3

Updating

```cpp
ahead = curr;
```

inside the loop over `buy`.

Wrong:

```cpp
for(buy){

    ...

    ahead=curr;
}
```

This mixes current-day states with next-day states.

Correct:

```cpp
Compute

curr[0]

curr[1]

↓

ahead=curr
```

---

# Complexity

## DP

Time

```txt
O(n)
```

Space

```txt
O(n)
```

---

## Two Arrays

Time

```txt
O(n)
```

Space

```txt
O(1)
```

(two arrays of size 2)

---

## Four Variables

Time

```txt
O(n)
```

Space

```txt
O(1)
```

---

# Greedy vs DP

| Greedy                                                         | DP                                             |
| -------------------------------------------------------------- | ---------------------------------------------- |
| Simpler                                                        | General framework                              |
| O(1) Space                                                     | O(1) after optimization                        |
| Uses positive differences                                      | Uses state transitions                         |
| Works only because unlimited transactions have no fee/cooldown | Easily extends to Stock III, IV, Cooldown, Fee |

---

# Pattern Recognition

Whenever you see:

```txt
Unlimited Transactions

Buy

Sell

Buy

Sell
```

Think:

```txt
Greedy

↓

Collect every positive increase.
```

If solving using DP:

```txt
State

dp[i][buy]

Buy State

↓

Buy

or

Skip

Sell State

↓

Sell

or

Hold
```

---

# Interview Trigger

```txt
If you see:

Unlimited stock transactions

No cooldown

No fee

Think:

Greedy

↓

Sum all positive price differences.

If interviewer asks for DP:

State:

dp[i][buy]

Transitions:

Buy

↓

max(
Buy,
Skip
)

Sell

↓

max(
Sell,
Hold
)

Optimize:

Tabulation

↓

Two Arrays

↓

Four Variables
```

---

# Stock DP Framework

This problem introduces the **core stock state machine** used in all advanced stock problems.

| Problem         | Additional State                           |
| --------------- | ------------------------------------------ |
| Stock I         | No DP needed (Greedy)                      |
| Stock II        | `day`, `buy`                               |
| Stock III       | `day`, `buy`, `transactionsLeft = 2`       |
| Stock IV        | `day`, `buy`, `transactionsLeft = k`       |
| Cooldown        | `day`, `buy` (modified sell transition)    |
| Transaction Fee | `day`, `buy` (fee deducted during selling) |

### Key Insight

The **buy/sell transitions never change**:

```txt
Buy State

↓

Buy

or

Skip

Sell State

↓

Sell

or

Hold
```

Only the **state gains extra dimensions** (transactions, cooldown, fee) in later problems. Once you understand this two-state DP, the rest of the stock DP series becomes a natural extension rather than entirely new problems.
