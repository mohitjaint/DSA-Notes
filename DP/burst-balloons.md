# Burst Balloons

## References

**Question:**

[https://leetcode.com/problems/burst-balloons/](https://leetcode.com/problems/burst-balloons/)

**Related Problems:**

- Matrix Chain Multiplication
- Minimum Cost to Cut a Stick
- Boolean Parenthesization
- Minimum Score Triangulation of Polygon

---

# Pattern

```text
Partition DP

Interval DP

MCM Variation
```

---

# Problem Statement

Given an array of balloons, bursting balloon `i` gives

```text
left × current × right
```

coins, where `left` and `right` are the nearest remaining balloons.

Find the maximum coins obtainable.

---

# Key Observation

Trying to decide the **first balloon** to burst is difficult because the neighbours keep changing.

Instead,

think in reverse.

Choose the **last balloon** to burst.

---

# Why Choose the Last Balloon?

Suppose the interval is

```text
i ............ j
```

If balloon `k` is burst **last**,

then every other balloon inside the interval has already disappeared.

Therefore,

the neighbours of `k` are fixed.

```text
nums[i-1]   nums[k]   nums[j+1]
```

The gain becomes

```text
nums[i-1] × nums[k] × nums[j+1]
```

which is independent of the order inside the left and right intervals.

This makes the problem a perfect Partition DP.

---

# Boundary Trick

Before solving,

add virtual balloons.

```cpp
nums.insert(nums.begin(),1);
nums.push_back(1);
```

Example

```text
Original

3 1 5 8

↓

1 3 1 5 8 1
```

The virtual balloons are never burst.

They simply provide fixed neighbours.

---

# DP State

```cpp
dp[i][j]
```

Meaning

```text
Maximum coins

obtained by bursting

all balloons

between i and j.
```

Both

```text
i

and

j
```

are indices in the modified array.

---

# Base Case

```cpp
if(i > j)
    return 0;
```

Reason

```text
No balloons left.

No coins.
```

---

# Transition

Choose every balloon as the **last balloon**.

```text
i ... k ... j
```

Coins obtained

```text
Current Gain

+

Left Interval

+

Right Interval
```

Transition

```cpp
for(int k=i;k<=j;k++){

    coins=

    nums[i-1]*nums[k]*nums[j+1]

    +

    dp[i][k-1]

    +

    dp[k+1][j];
}
```

Take

```cpp
max(coins)
```

---

# Why

```cpp
nums[i-1] * nums[k] * nums[j+1]
```

Suppose

```text
1 3 1 5 8 1
```

Current interval

```text
3 1 5
```

If

```text
1
```

is burst last,

then

```text
3

and

5
```

have already disappeared.

The neighbours become

```text
1

1

8
```

Therefore,

coins earned are

```text
1 × 1 × 8
```

Exactly

```cpp
nums[i-1] * nums[k] * nums[j+1]
```

---

# Relation with MCM

| Matrix Chain Multiplication        | Burst Balloons            |
| ---------------------------------- | ------------------------- |
| Interval = matrices                | Interval = balloons       |
| Choose partition `k`               | Choose last balloon `k`   |
| `dp[i][k] + dp[k+1][j]`            | `dp[i][k-1] + dp[k+1][j]` |
| Merge cost = matrix multiplication | Merge cost = coins earned |

The recurrence has the same Partition DP structure.

---

# Memoization

```text
Solve(i,j)

↓

Choose last balloon

↓

Left Interval

+

Right Interval

+

Current Coins

↓

Take Maximum
```

---

# Tabulation

## DP Dependency

```cpp
dp[i][j]
```

depends on

```cpp
dp[i][k-1]

dp[k+1][j]
```

Notice

```text
k+1 > i
```

Therefore,

rows below must already be computed.

---

# Iteration Order (Striver's Method)

```cpp
for(int i=n;i>=1;i--){

    for(int j=i;j<=n;j++){

        ...
    }
}
```

This guarantees all dependent states are already available.

---

# Gap Method

The DP table can also be filled diagonal by diagonal.

```cpp
for(int gap=0;gap<n;gap++){

    for(int i=1;i+gap<=n;i++){

        int j=i+gap;

        ...
    }
}
```

Where

```text
gap = j-i
```

This computes smaller intervals before larger ones.

Both approaches are equivalent.

---

# Complexity

Let

```text
n = number of balloons
```

Time

```text
O(n³)
```

Space

```text
O(n²)
```

---

# Common Mistake #1

Trying to choose the **first** balloon.

Neighbours are changing continuously.

Choosing the **last** balloon fixes the neighbours.

---

# Common Mistake #2

Forgetting to add

```text
1

at both ends.
```

Without virtual balloons,

the boundary cases become complicated.

---

# Common Mistake #3

Using

```cpp
nums[k-1] * nums[k] * nums[k+1]
```

Incorrect.

After many balloons disappear,

these are **not** necessarily the neighbours.

Correct

```cpp
nums[i-1] * nums[k] * nums[j+1]
```

because `k` is burst last.

---

# Common Mistake #4

Using

```cpp
dp[i][k]

+

dp[k][j]
```

Correct recurrence

```cpp
dp[i][k-1]

+

dp[k+1][j]
```

because balloon `k` is removed in the current step.

---

# Pattern Recognition

Whenever you encounter

```text
Choose one element

↓

It divides the interval

↓

Left and Right become independent

↓

Current gain depends only on boundaries
```

think

```text
Partition DP

↓

Choose the LAST operation

↓

Solve left

+

Solve right

↓

Merge
```

---

# Interview Trigger

```text
Burst Balloons

↓

Choosing first balloon is difficult

↓

Choose LAST balloon

↓

Add virtual balloons (1, 1)

↓

State

dp[i][j]

=

Maximum coins from interval

↓

Try every last balloon

↓

Current Coins

+

Left

+

Right

↓

Take Maximum

↓

Reverse i

OR

Gap Method

↓

O(n³)
```

---

# Core Insight

The breakthrough is to **reverse the perspective**. Instead of deciding which balloon to burst first (where neighbours keep changing), decide which balloon is **burst last** in an interval. At that moment, all other balloons inside the interval have already disappeared, so the neighbours are fixed and known: `nums[i-1]` and `nums[j+1]`. This transforms a seemingly dynamic process into a clean **Partition DP**, making the recurrence almost identical to Matrix Chain Multiplication.
