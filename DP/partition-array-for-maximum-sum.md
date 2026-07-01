# Partition Array for Maximum Sum

## References

**Question:**

[https://leetcode.com/problems/partition-array-for-maximum-sum/](https://leetcode.com/problems/partition-array-for-maximum-sum/)

**Related Problems:**

* Palindrome Partitioning II
* Rod Cutting
* Integer Break
* Word Break (State Similarity)

---

# Pattern

```text
1D DP

Suffix DP

Partition DP
```

---

# Problem Statement

Partition the array into contiguous subarrays.

Each partition

* has length **at most `k`**
* contributes

```text
Maximum Element × Length of Partition
```

Return the maximum possible sum.

---

# Key Observation

Unlike Matrix Chain Multiplication,

there is **no interaction between partitions**.

Once the first partition is fixed,

its contribution is completely determined.

Only the remaining suffix needs to be solved.

Therefore,

```text
Suffix DP
```

instead of Interval DP.

---

# DP State

```cpp
dp[i]
```

Meaning

```text
Maximum sum obtainable

from index i

to the end.
```

---

# Base Case

```cpp
dp[n] = 0;
```

Meaning

```text
Empty suffix

contributes

0.
```

---

# Transition

Suppose the first partition starts at

```text
i
```

Its length can be

```text
1

2

...

k
```

For every possible ending index

```text
j
```

compute

```text
Current Partition Contribution

+

Best Answer for Remaining Suffix
```

Transition

```cpp
dp[i] = max(
    CurrentMax * Length
    + dp[j+1]
)
```

---

# Maintaining Maximum Efficiently

Do **not** compute

```cpp
max_element(...)
```

inside the loop.

Instead,

maintain the maximum while expanding the partition.

```cpp
currMax = max(currMax, arr[j]);
```

This avoids an extra O(k) computation.

---

# Memoization

State

```cpp
f(i)
```

Transition

```cpp
currMax = INT_MIN;

for(int j=i;
    j<min(n,i+k);
    j++){

    currMax=max(currMax,arr[j]);

    ans=max(
        ans,
        currMax*(j-i+1)
        +f(j+1)
    );
}
```

---

# Tabulation

Since

```cpp
dp[i]
```

depends only on

```cpp
dp[j+1]
```

where

```text
j+1 > i
```

iterate backwards.

```cpp
for(int i=n-1;i>=0;i--)
```

---

# Tabulation Transition

```cpp
currMax = INT_MIN;

for(int j=i;
    j<min(n,i+k);
    j++){

    currMax=max(currMax,arr[j]);

    dp[i]=max(
        dp[i],
        currMax*(j-i+1)
        +dp[j+1]
    );
}
```

---

# Complexity

States

```text
O(n)
```

Transitions per state

```text
At most k
```

Overall

```text
Time : O(nk)

Space : O(n)
```

---

# Common Mistake #1

Misinterpreting

```text
k
```

Wrong

```text
Maximum number of partitions
```

Correct

```text
Maximum size of each partition
```

---

# Common Mistake #2

Using state

```cpp
dp[i][partitions]
```

There is **no restriction on the number of partitions**.

The only state needed is

```cpp
dp[i]
```

---

# Common Mistake #3

Using

```cpp
max_element(...)
```

inside the transition.

Wrong Complexity

```text
O(nk²)
```

Maintain

```cpp
currMax
```

instead.

---

# Common Mistake #4

Writing

```cpp
int currMax = max(currMax, arr[j]);
```

inside the loop.

This creates a **new local variable** and shadows the outer one.

Correct

```cpp
currMax = max(currMax, arr[j]);
```

---

# Common Mistake #5

Iterating beyond partition size.

Wrong

```cpp
for(j=i;j<n;j++)
```

Correct

```cpp
for(j=i;
    j<min(n,i+k);
    j++)
```

---

# Why This is Not Classical Partition DP

Classical Partition DP

```text
Choose Partition

↓

Left Subproblem

+

Right Subproblem
```

Example

* Matrix Chain Multiplication
* Burst Balloons
* Minimum Cost to Cut Stick

State

```cpp
dp[i][j]
```

---

This problem

```text
Choose First Partition

↓

Contribution Fixed

↓

Only Remaining Suffix
```

State

```cpp
dp[i]
```

There is **no left recursive call**.

---

# Pattern Recognition

Whenever you see

```text
Choose first segment

↓

Its contribution becomes fixed

↓

Only remaining suffix matters
```

think

```text
1D Suffix DP
```

instead of

```text
Interval DP
```

---

# Comparison with Palindrome Partitioning II

| Palindrome Partitioning II | Partition Array for Maximum Sum |
| -------------------------- | ------------------------------- |
| Choose palindrome prefix   | Choose partition length         |
| Prefix becomes fixed       | Partition becomes fixed         |
| Solve remaining suffix     | Solve remaining suffix          |
| `dp[i]`                    | `dp[i]`                         |
| O(n²)                      | O(nk)                           |

---

# Interview Trigger

```text
Partition Array for Maximum Sum

↓

Suffix DP

↓

State

dp[i]

↓

Choose partition length

1...k

↓

Maintain current maximum

↓

Current Contribution

+

dp[next]

↓

Take Maximum

↓

O(nk)
```

---

# Core Insight

Although the problem involves partitioning an array, it is **not** a classical interval Partition DP. Once the first partition is chosen, its contribution is completely determined and never changes. Therefore, there is no need to recurse on the left part—only the remaining suffix matters. This reduces the state to a simple **1D suffix DP (`dp[i]`)**, where each state tries all valid partition lengths (up to `k`) and combines the current partition's contribution with the optimal answer for the remaining suffix. Maintaining the partition maximum incrementally yields the optimal **O(nk)** solution.
