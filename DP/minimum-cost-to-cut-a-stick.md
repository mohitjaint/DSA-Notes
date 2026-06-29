# Minimum Cost to Cut a Stick

## References

**Question:**

[https://leetcode.com/problems/minimum-cost-to-cut-a-stick/](https://leetcode.com/problems/minimum-cost-to-cut-a-stick/)

**Related Problems:**

- Matrix Chain Multiplication
- Burst Balloons
- Boolean Parenthesization
- Palindrome Partitioning II
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

Given a stick of length `n` and an array of cut positions, perform all cuts in any order.

Each cut costs the **current length of the stick being cut**.

Find the **minimum total cost**.

---

# Key Observation

The order of cuts changes the total cost.

Example

```text
Stick : 0 ----------- 7

Cuts : 1 3 4 5
```

Cutting

```text
3 first
```

and

```text
1 first
```

produce different costs.

Therefore,

we must try **every possible first cut**.

---

# The Trick

Add the stick boundaries.

```cpp
cuts.push_back(0);
cuts.push_back(n);
sort(cuts.begin(), cuts.end());
```

Example

```text
Original

1 3 4 5

↓

After adding boundaries

0 1 3 4 5 7
```

Now every stick interval is represented by two indices.

---

# Why Add `0` and `n`?

Without boundaries,

```text
Current Stick

0 -------- 7
```

has no representation inside the cuts array.

Adding

```text
0

and

n
```

makes every interval look identical.

Example

```text
cuts

0 1 3 4 5 7
```

Now

```text
f(0,5)
```

means

```text
Stick

0 -----------7
```

while

```text
f(2,5)
```

means

```text
Stick

3 -----------7
```

No special handling is needed.

---

# DP State

```cpp
dp[i][j]
```

Meaning

```text
Minimum cost

to perform all cuts

strictly between

cuts[i]

and

cuts[j].
```

Notice

```text
i

and

j
```

are **indices**, not stick positions.

---

# Base Case

```cpp
if(j - i <= 1)
    return 0;
```

Reason

```text
No cut exists

between

cuts[i]

and

cuts[j].
```

Example

```text
0 ----- 3
```

Nothing to cut.

---

# Transition

Try every possible **first cut**.

```text
cuts[i]

|

cuts[k]

|

cuts[j]
```

Cost

```text
Current Stick Length

+

Left Interval

+

Right Interval
```

Transition

```cpp
for(int k=i+1;k<j;k++){

    cost=min(
        cost,
        cuts[j]-cuts[i]
        + dp[i][k]
        + dp[k][j]
    );
}
```

---

# Why `dp[k][j]` instead of `dp[k+1][j]`?

Unlike MCM,

the cut at `k` becomes the boundary of **both** new sticks.

Example

```text
0 --------4--------7
```

Left stick

```text
0 → 4
```

Right stick

```text
4 → 7
```

Therefore

```cpp
Left

dp[i][k]

Right

dp[k][j]
```

The boundary is shared.

---

# Relation with MCM

| Matrix Chain Multiplication        | Stick Cutting             |
| ---------------------------------- | ------------------------- |
| State = matrices `i...j`           | State = cuts `i...j`      |
| Partition at `k`                   | First cut at `k`          |
| `dp[i][k] + dp[k+1][j]`            | `dp[i][k] + dp[k][j]`     |
| Merge cost = matrix multiplication | Merge cost = stick length |

The overall DP structure is identical.

---

# Memoization

```cpp
f(i,j)

↓

Try every first cut

↓

Left Interval

+

Right Interval

+

Current Stick Length

↓

Take Minimum
```

---

# Tabulation

## DP Dependency

```cpp
dp[i][j]
```

depends on

```cpp
dp[i][k]

dp[k][j]
```

Notice

```text
k > i
```

Therefore

```text
Rows below

must already be computed.
```

---

# Iteration Order (Striver's Method)

```cpp
for(int i=m-1;i>=0;i--){

    for(int j=i+2;j<m;j++){

        ...
    }
}
```

---

## Why `j = i + 2`?

If

```text
j = i+1
```

then there is

```text
No cut

between

cuts[i]

and

cuts[j].
```

Example

```text
0 --------3
```

Cost

```text
0
```

The first meaningful interval always contains **at least one cut**, so

```cpp
j = i + 2;
```

---

# Gap Method (Diagonal Filling)

Instead of filling row by row,

fill the DP table diagonal by diagonal.

Suppose

```text
m = 6
```

DP table

```text
      0   1   2   3   4   5

0     ●   ●   1   2   3   4

1         ●   ●   1   2   3

2             ●   ●   1   2

3                 ●   ●   1

4                     ●   ●

5                         ●
```

Where

```text
●

=

Base Case
```

---

### Gap Iteration

```cpp
for(int gap=2;gap<m;gap++){

    for(int i=0;i+gap<m;i++){

        int j=i+gap;

        ...
    }
}
```

This computes intervals in increasing order of size.

---

# Complexity

Let

```text
m = cuts.size() + 2
```

Time

```text
O(m³)
```

Space

```text
O(m²)
```

---

# Common Mistake #1

Not adding

```text
0

and

n
```

Without them,

the interval state is not uniform.

---

# Common Mistake #2

Using stick positions as DP states.

Incorrect

```cpp
f(leftPosition,rightPosition)
```

Correct

```cpp
f(leftBoundaryIndex,rightBoundaryIndex)
```

---

# Common Mistake #3

Writing

```cpp
dp[i][k]

+

dp[k+1][j]
```

This is **MCM**.

Correct here

```cpp
dp[i][k]

+

dp[k][j]
```

because the cut is the common boundary.

---

# Common Mistake #4

Using

```cpp
j=i+1
```

in tabulation.

Those intervals contain no cuts.

Start from

```cpp
j=i+2
```

or

```cpp
gap=2
```

---

# Pattern Recognition

Whenever a problem says

```text
Choose the first operation

↓

Split into two independent intervals

↓

Pay current interval cost
```

think

```text
Partition DP

↓

Interval State

↓

Try every partition

↓

Left

+

Right

+

Current Cost

↓

Minimum / Maximum
```

---

# Interview Trigger

```text
Minimum Cost to Cut a Stick

↓

Partition DP

↓

Add 0 and n

↓

State

dp[i][j]

=

Minimum cost between boundaries

cuts[i]

and

cuts[j]

↓

Try every first cut

↓

Current Stick Length

+

Left

+

Right

↓

Take Minimum

↓

Reverse i

OR

Gap Method

↓

O(m³)
```

---

# Core Insight

The crucial transformation is **adding the stick boundaries (`0` and `n`)** and defining the DP state using **boundary indices instead of stick coordinates**. This converts the problem into a clean **interval DP**, where each state represents a stick segment bounded by two cuts. The recurrence then becomes almost identical to **Matrix Chain Multiplication**: try every possible first partition (cut), solve the left and right intervals independently, add the current interval cost, and choose the minimum. This boundary-index representation is what makes memoization and tabulation straightforward.
