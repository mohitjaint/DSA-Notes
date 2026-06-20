## References

Question:

[https://leetcode.com/problems/best-time-to-buy-and-sell-stock/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

Related Problems:

[https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

[https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)

[https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/)

[https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

[https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)

---

# Pattern

```txt
Greedy

Prefix Minimum

Stock DP Introduction
```

---

# Problem Statement

You may perform **at most one transaction**.

One transaction means:

```txt
Buy once

↓

Sell once
```

Find the maximum possible profit.

---

# Key Observation

For every selling day:

```txt
Profit

=

Selling Price

-

Buying Price
```

To maximize profit:

```txt
Selling Price

-

Minimum Buying Price
```

So while traversing the array:

- Keep track of the **minimum price seen so far**.
- Compute profit if sold today.
- Update the maximum profit.

---

# Algorithm

Maintain:

```cpp
mini
```

Meaning:

```txt
Minimum stock price
seen till now.
```

For every day:

```cpp
profit = max(profit, prices[i] - mini);
```

Then update:

```cpp
mini = min(mini, prices[i]);
```

---

# Why Does It Work?

Suppose today's price is:

```txt
8
```

Earlier prices:

```txt
7 2 5 3
```

Best buying price is:

```txt
2
```

Profit:

```txt
8 - 2 = 6
```

There is no reason to buy at:

```txt
7

or

5

or

3
```

Therefore only the **minimum prefix price** matters.

---

# Correct Algorithm

```cpp
int mini = prices[0];
int profit = 0;

for(int i = 1; i < prices.size(); i++){

    profit = max(profit, prices[i] - mini);

    mini = min(mini, prices[i]);
}

return profit;
```

---

# Complexity

Time:

```txt
O(n)
```

Space:

```txt
O(1)
```

---

# Relation to Stock DP

Although this solution is greedy, it introduces the idea of maintaining two states.

Later stock problems use DP with states like:

```txt
Day

Buy / Sell

Transactions Left
```

For **Stock I**, the DP simplifies to tracking:

```txt
Minimum buying price

Maximum profit
```

which is why a greedy solution is sufficient.

---

# Pattern Recognition

Whenever you see:

```txt
One Buy

One Sell

Maximum Profit
```

Think:

```txt
Maintain

Minimum Price

+

Maximum Profit
```

No DP is required.

---

# Interview Trigger

```txt
If you see:

Only one transaction allowed

Think:

For every selling day,

find the minimum buying price before it.

Maintain:

Minimum Prefix

+

Maximum Profit

Time: O(n)

Space: O(1)
```

### Connection to the Stock DP Series

This problem is the **base case** of all stock DP problems.

| Problem                    | Technique               |
| -------------------------- | ----------------------- |
| Stock I                    | Greedy (Prefix Minimum) |
| Stock II                   | DP / State Machine      |
| Stock III                  | DP (2 Transactions)     |
| Stock IV                   | DP (k Transactions)     |
| Stock with Cooldown        | DP                      |
| Stock with Transaction Fee | DP                      |

Once you move beyond **one transaction**, the greedy approach is no longer sufficient, and the full stock DP framework becomes necessary.
