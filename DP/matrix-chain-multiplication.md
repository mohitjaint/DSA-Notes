# Matrix Chain Multiplication (MCM)

## References

**Question:**

[https://www.geeksforgeeks.org/problems/matrix-chain-multiplication0303/1](https://www.geeksforgeeks.org/problems/matrix-chain-multiplication0303/1)

**Related Problems:**

- Burst Balloons
- Minimum Cost to Cut a Stick
- Boolean Parenthesization
- Palindrome Partitioning II
- Partition Array for Maximum Sum

---

# Pattern

```text
Partition DP

Interval DP

Split DP
```

---

# Problem Statement

Given an array

```text
arr = [10, 20, 30, 40]
```

Matrices are

```text
A1 = 10 × 20

A2 = 20 × 30

A3 = 30 × 40
```

Find the **minimum number of scalar multiplications** required to multiply all matrices.

---

# Key Observation

The order of multiplication changes the total cost.

Example

```text
(A1A2)A3

≠

A1(A2A3)
```

Therefore,

we must try **every possible partition**.

---

# DP State

```cpp
dp[i][j]
```

Meaning

```text
Minimum multiplication cost

to multiply matrices

Ai ... Aj
```

---

# Base Case

```cpp
if(i == j)
    return 0;
```

Reason

```text
One matrix

↓

No multiplication required.
```

---

# Transition

Try every partition.

```text
Ai ... Ak

|

Ak+1 ... Aj
```

Transition

```cpp
for(int k=i;k<j;k++){

    cost =

    dp[i][k]

    +

    dp[k+1][j]

    +

    arr[i-1]*arr[k]*arr[j];
}
```

Take

```cpp
min(cost)
```

---

# Why

```cpp
arr[i-1] * arr[k] * arr[j]
```

Suppose

```text
A1 : 10×20

A2 : 20×30

A3 : 30×40
```

Then

```text
arr

10 20 30 40
```

Partition

```text
(A1A2)

|

(A3)
```

Left matrix

```text
10 × 30
```

Right matrix

```text
30 × 40
```

Final multiplication cost

```text
10 × 30 × 40
```

which equals

```cpp
arr[i-1] * arr[k] * arr[j]
```

This formula is the heart of MCM.

---

# Memoization

```cpp
f(i,j)

↓

Try every k

↓

Left

+

Right

+

Current Cost

↓

Minimum
```

---

# Tabulation

## DP Dependencies

```cpp
dp[i][j]
```

depends on

```cpp
dp[i][k]

dp[k+1][j]
```

Notice

```text
Left interval

↓

Smaller

Right interval

↓

Smaller
```

Therefore,

**smaller intervals must be computed first.**

---

# Method 1 : Reverse `i` (Striver's Method)

Iteration

```cpp
for(int i=n-1;i>=1;i--){

    for(int j=i+1;j<n;j++){

        ...
    }
}
```

### Why does it work?

Current state

```text
dp[i][j]
```

depends on

```text
dp[k+1][j]
```

where

```text
k+1 > i
```

Therefore,

rows having larger

```text
i
```

must already be computed.

Hence

```text
i

↓

n-1

↓

1
```

For the same row,

```text
dp[i][k]
```

has

```text
k < j
```

so

```text
j

↓

Left → Right
```

works naturally.

---

# Method 2 : Gap (Diagonal) Method

Instead of rows,

fill the table **diagonal by diagonal**.

Suppose

```text
n = 5
```

DP table

```text
      1   2   3   4

1     ●   1   2   3

2         ●   1   2

3             ●   1

4                 ●
```

Where

```text
●

=

Base Case

(i==j)
```

The numbers indicate the order of computation.

---

## Gap = 1

Compute

```text
dp[1][2]

dp[2][3]

dp[3][4]
```

---

## Gap = 2

Compute

```text
dp[1][3]

dp[2][4]
```

---

## Gap = 3

Compute

```text
dp[1][4]
```

---

## Gap Iteration

```cpp
for(int gap=1;gap<=n-2;gap++){

    for(int i=1;i+gap<n;i++){

        int j=i+gap;

        ...
    }
}
```

Equivalent Length version

```cpp
for(int len=2;len<n;len++){

    for(int i=1;i+len-1<n;i++){

        int j=i+len-1;

        ...
    }
}
```

---

# Why Gap Method Works

Every interval depends only on **smaller intervals**.

Example

```text
dp[1][4]
```

depends on

```text
dp[1][1]

dp[2][4]

dp[1][2]

dp[3][4]

dp[1][3]

dp[4][4]
```

All of these have **smaller gaps**,

so they have already been computed.

---

# Which Method Should You Use?

| Reverse `i`                        | Gap Method                   |
| ---------------------------------- | ---------------------------- |
| Used by Striver                    | Common in books & editorials |
| Derived directly from dependencies | Derived from interval length |
| Easy after writing memoization     | Easy to visualize            |
| Bottom-to-top row filling          | Diagonal filling             |

Both have

```text
Time : O(n³)

Space : O(n²)
```

They are simply **two valid topological orders** of the same DP dependency graph.

---

# Complexity

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

Using

```cpp
for(int i=1;i<n;i++)
```

This computes larger intervals before their dependencies.

Always use

```text
Reverse i

OR

Gap Method
```

---

# Common Mistake #2

Wrong partition loop

Correct

```cpp
for(int k=i;k<j;k++)
```

Not

```cpp
k<=j
```

---

# Common Mistake #3

Wrong multiplication formula

Correct

```cpp
arr[i-1] * arr[k] * arr[j]
```

Remember

```text
Left Result

↓

arr[i-1] × arr[k]

Right Result

↓

arr[k] × arr[j]
```

---

# Common Mistake #4

Returning

```cpp
dp[0][n-1]
```

Matrices start from

```text
1
```

Correct answer

```cpp
dp[1][n-1]
```

---

# Pattern Recognition

Whenever you see words like

```text
Split

Partition

Cut

Burst

Parenthesize

Divide
```

think

```text
Partition DP

↓

State = Interval

↓

Try every partition

↓

Left

+

Right

+

Merge Cost

↓

Min / Max
```

---

# Interview Trigger

```text
Matrix Chain Multiplication

↓

Interval DP

↓

dp[i][j]

=

Minimum cost

to multiply

Ai...Aj

↓

Try every partition k

↓

Left Cost

+

Right Cost

+

Current Multiplication Cost

↓

Take Minimum

↓

Tabulation

↓

Reverse i (Striver)

OR

Gap Method

↓

O(n³)
```

---

# Core Insight

Matrix Chain Multiplication is the **foundation of Partition DP**. Unlike previous DP patterns (Take/Not Take, LIS, Grid DP), the decision is **where to split the interval**. Every state represents an interval, every transition tries **all possible partition points**, recursively solves the left and right intervals, and combines them with a merge cost. Once you understand this recurrence, the same template applies almost directly to problems like **Burst Balloons**, **Minimum Cost to Cut a Stick**, **Boolean Parenthesization**, and **Palindrome Partitioning II**.
