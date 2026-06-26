# Number of Longest Increasing Subsequences

## References

**Question:**

[https://leetcode.com/problems/number-of-longest-increasing-subsequence/](https://leetcode.com/problems/number-of-longest-increasing-subsequence/)

**Related Problems:**

- Longest Increasing Subsequence
- Print Longest Increasing Subsequence
- Longest Bitonic Subsequence

---

# Pattern

```text
LIS Variation

1D DP

Counting DP
```

---

# Problem Statement

Given an integer array, return the **number of Longest Increasing Subsequences (LIS)**.

Unlike the normal LIS problem, we need to count **how many** longest subsequences exist.

---

# Key Observation

In the standard LIS DP,

```cpp
dp[i]
```

stores

```text
Length of the LIS

ending at index i.
```

To count the number of LIS, maintain another array.

```cpp
cnt[i]
```

Meaning

```text
Number of LIS

of length dp[i]

ending at index i.
```

---

# DP State

## Length DP

```cpp
dp[i]
```

```text
Length of LIS

ending at i.
```

---

## Count DP

```cpp
cnt[i]
```

```text
Number of LIS

ending at i

having length dp[i].
```

---

# Initialization

Every element alone forms an LIS.

```cpp
dp[i] = 1;
cnt[i] = 1;
```

---

# Transition

For every previous index

```cpp
j < i
```

if

```cpp
nums[j] < nums[i]
```

then two cases arise.

---

## Case 1 : Better LIS Found

```cpp
if(dp[j] + 1 > dp[i])
```

We found a longer subsequence.

Update

```cpp
dp[i] = dp[j] + 1;
cnt[i] = cnt[j];
```

### Why?

The previous best length is discarded.

All optimal sequences now come through `j`.

---

## Case 2 : Another LIS of Same Length

```cpp
else if(dp[j] + 1 == dp[i])
```

Another way to obtain the same optimal length.

```cpp
cnt[i] += cnt[j];
```

### Why?

Every LIS ending at `j` can be extended to `i`.

Hence all of them contribute.

---

# Finding the Answer

Find the maximum LIS length.

```cpp
maxLen
```

Then sum the counts.

```cpp
for(int i=0;i<n;i++){

    if(dp[i]==maxLen)
        ans+=cnt[i];
}
```

---

# Example

```text
1 3 5 4 7
```

DP

| Index | Value |  dp | cnt |
| ----: | ----: | --: | --: |
|     0 |     1 |   1 |   1 |
|     1 |     3 |   2 |   1 |
|     2 |     5 |   3 |   1 |
|     3 |     4 |   3 |   1 |
|     4 |     7 |   4 |   2 |

Answer

```text
2
```

LIS are

```text
1 3 5 7

1 3 4 7
```

---

# Complexity

Time

```text
O(n²)
```

Space

```text
O(n)
```

---

# Relation with LIS

Normal LIS

```cpp
dp[i]
```

stores

```text
Best length.
```

This problem simply adds

```cpp
cnt[i]
```

to keep track of

```text
How many ways

to obtain that best length.
```

---

# Common Mistake #1

Writing

```cpp
cnt[i]++;
```

when another optimal predecessor is found.

Incorrect.

Different predecessors may themselves have multiple LIS.

Correct

```cpp
cnt[i] += cnt[j];
```

---

# Common Mistake #2

Updating

```cpp
cnt[i]
```

before updating

```cpp
dp[i]
```

Always update the length first.

---

# Common Mistake #3

When finding a longer LIS,

writing

```cpp
cnt[i] += cnt[j];
```

Incorrect.

A longer LIS replaces all previous shorter ones.

Correct

```cpp
cnt[i] = cnt[j];
```

---

# Pattern Recognition

Whenever a DP problem asks

```text
Maximum

AND

Number of ways
```

think

```text
One DP array

for the optimal value

+

One DP array

for the number of optimal solutions.
```

---

# Interview Trigger

```text
Number of LIS

↓

Standard LIS DP

↓

dp[i]

=

Length of LIS ending at i

↓

Need count

↓

cnt[i]

=

Number of LIS ending at i

↓

Longer sequence found

↓

Replace count

↓

Equal length found

↓

Add count

↓

Sum counts of maximum length

↓

O(n²)
```

---

# Core Insight

This problem extends the classic LIS DP by storing **two pieces of information** for every state:

- **`dp[i]`**: the best (maximum) LIS length ending at `i`.
- **`cnt[i]`**: the number of ways to achieve that best length.

Whenever a better length is found, the count is **replaced**. Whenever another sequence achieves the same optimal length, the count is **added**. This "best value + number of optimal ways" pattern is one of the most common extensions of dynamic programming.
