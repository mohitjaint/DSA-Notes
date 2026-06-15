# Rod Cutting

## References

Question:

[https://www.geeksforgeeks.org/problems/rod-cutting0840/1](https://www.geeksforgeeks.org/problems/rod-cutting0840/1)

Related Problems:

[https://www.geeksforgeeks.org/problems/knapsack-with-duplicate-items4201/1](https://www.geeksforgeeks.org/problems/knapsack-with-duplicate-items4201/1)

[https://leetcode.com/problems/coin-change/](https://leetcode.com/problems/coin-change/)

[https://leetcode.com/problems/coin-change-ii/](https://leetcode.com/problems/coin-change-ii/)

[https://www.geeksforgeeks.org/problems/0-1-knapsack-problem0945/1](https://www.geeksforgeeks.org/problems/0-1-knapsack-problem0945/1)

---

# Pattern

```txt
Unbounded Knapsack

Maximize Profit

Unlimited Reuse Allowed

DP on Length
```

---

# Key Observation

We are given:

```txt
Rod Length = n

price[i]
=
Price of piece of length (i+1)
```

We can cut the rod into:

```txt
Any number of pieces
```

and:

```txt
A piece length can be used multiple times
```

This immediately suggests:

```txt
Unbounded Knapsack
```

---

# Problem Transformation

Map Rod Cutting to Unbounded Knapsack:

| Rod Cutting  | Unbounded Knapsack |
| ------------ | ------------------ |
| Piece Length | Weight             |
| Piece Price  | Value              |
| Rod Length   | Capacity           |
| Cut Again    | Reuse Item         |

---

# Constraint Analysis

```txt
Rod Length = n
```

State:

```txt
Piece Index
Remaining Length
```

Therefore:

```txt
DP ✓
```

---

# Why Greedy Fails

Example:

```txt
Length : 1 2 3 4

Price  : 2 5 7 8
```

Rod Length:

```txt
4
```

Greedy:

```txt
Take length 4

Profit = 8
```

Optimal:

```txt
2 + 2

Profit = 10
```

Therefore:

```txt
Greedy ✗
DP ✓
```

---

# Bruteforce

For every piece:

```txt
Take
Take Again
Take Again
...
Not Take
```

Try all possibilities.

Complexity:

```txt
Exponential
```

---

# State

```cpp
dp[i][len]
```

Meaning:

```txt
Maximum profit obtainable

using piece lengths [1...(i+1)]

to form rod length = len
```

---

# Base Case

Only piece length:

```txt
1
```

available.

For rod length:

```txt
len
```

we can cut:

```txt
len times
```

Therefore:

```cpp
dp[0][len]
=
len * price[0]
```

---

## Example

```txt
price[0] = 3

Rod Length = 5
```

Profit:

```txt
5 × 3 = 15
```

---

# Transition

## Not Take

Ignore current piece length:

```cpp
dp[i-1][len]
```

---

## Take

Take one piece of length:

```cpp
i+1
```

Profit gained:

```cpp
price[i]
```

Remaining rod:

```cpp
len - (i+1)
```

Since the same piece can be used again:

```cpp
price[i]
+
dp[i][len-(i+1)]
```

Notice:

```cpp
dp[i]
```

not:

```cpp
dp[i-1]
```

---

# Final Transition

```cpp
dp[i][len]
=
max(
    dp[i-1][len],
    price[i]
    +
    dp[i][len-(i+1)]
)
```

---

# Why It Works

Every optimal solution:

```txt
Either uses current piece length
or does not use it
```

If it uses it:

```txt
Take one piece
+
Solve remaining rod
```

Since reuse is allowed:

```txt
Current piece remains available
```

Hence:

```cpp
dp[i][remaining]
```

is correct.

---

# Space Optimization

Observation:

```txt
dp[i][len]
depends on

dp[i-1][len]

and

dp[i][len-pieceLength]
```

Therefore:

```txt
1D DP Possible
```

---

# 1D State

```cpp
dp[len]
```

Meaning:

```txt
Maximum obtainable profit
for rod length = len
```

---

# Initialization

Using only piece length:

```txt
1
```

```cpp
for(int len=0; len<=n; len++)
{
    dp[len] = len * price[0];
}
```

---

# Transition

```cpp
dp[len]
=
max(
    dp[len],
    price[i]
    +
    dp[len-(i+1)]
)
```

---

# Iteration Order

```cpp
for(len = pieceLength;
    len <= n;
    len++)
```

Forward iteration.

---

# Why Forward Iteration?

Need:

```cpp
dp[len-pieceLength]
```

to already contain solutions that may have used:

```txt
Current Piece Length
```

multiple times.

Example:

```txt
Piece Length = 2

Rod Length = 8
```

Need:

```txt
8
↓
6
↓
4
↓
2
↓
0
```

which corresponds to:

```txt
2 + 2 + 2 + 2
```

Therefore:

```txt
Forward Iteration ✓
```

---

# Why Backward Iteration Fails?

Backward iteration prevents:

```txt
Reusing current piece
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
O(n²)
```

Space:

```txt
O(n²)
```

---

## 1D DP

Time:

```txt
O(n²)
```

Space:

```txt
O(n)
```

---

# Common Mistakes

### ❌ Using 0/1 Knapsack Transition

Wrong:

```cpp
price[i]
+
dp[i-1][len-(i+1)]
```

This allows using the piece only once.

---

### ❌ Using Backward Iteration

Wrong:

```cpp
for(len=n; len>=pieceLength; len--)
```

This converts the problem into:

```txt
0/1 Knapsack
```

---

### ❌ Wrong Base Case

Wrong:

```cpp
dp[0][len] = price[0]
```

Correct:

```cpp
dp[0][len]
=
len * price[0]
```

because piece length 1 can be used repeatedly.

---

### ❌ Comma Operator Instead of max()

Wrong:

```cpp
dp[len] =
(dp[len],
 price[i] + dp[len-pieceLength]);
```

This uses the C++ comma operator.

Equivalent to:

```cpp
dp[len]
=
price[i] + dp[len-pieceLength];
```

and completely ignores:

```cpp
dp[len]
```

Correct:

```cpp
dp[len]
=
max(
    dp[len],
    price[i] + dp[len-pieceLength]
);
```

---

# Pattern Recognition

Whenever you see:

```txt
Cut rod

Piece lengths

Piece prices

Maximum profit

Reuse pieces allowed
```

Think:

```txt
Unbounded Knapsack
```

---

# Related Problems

### Same Pattern

[https://www.geeksforgeeks.org/problems/knapsack-with-duplicate-items4201/1](https://www.geeksforgeeks.org/problems/knapsack-with-duplicate-items4201/1)

[https://leetcode.com/problems/coin-change/](https://leetcode.com/problems/coin-change/)

[https://leetcode.com/problems/coin-change-ii/](https://leetcode.com/problems/coin-change-ii/)

---

### Compare With

[https://www.geeksforgeeks.org/problems/0-1-knapsack-problem0945/1](https://www.geeksforgeeks.org/problems/0-1-knapsack-problem0945/1)

Difference:

```txt
0/1 Knapsack
→ dp[i-1][remaining]
→ Backward Iteration

Rod Cutting
→ dp[i][remaining]
→ Forward Iteration
```

---

# Interview Trigger

```txt
If you see:

Maximum Profit

Length + Price

Unlimited Cuts

Reuse Allowed

Think:

Unbounded Knapsack

Transition:

dp[i][len]
=
max(
    dp[i-1][len],
    price[i] + dp[i][len-(i+1)]
)

1D DP:
Forward Iteration
```
