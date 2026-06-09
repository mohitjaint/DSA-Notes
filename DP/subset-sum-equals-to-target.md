# DP 12: Subset Sum Equal to Target

## Problem

### [Link](https://www.geeksforgeeks.org/problems/subset-sum-problem-1611555638/1?utm_source=chatgpt.com)

Given an array:

```cpp
arr[]
```

and a target:

```cpp
target
```

Determine whether there exists a subset whose sum is exactly equal to `target`.

---

# Key Observation

For every element, there are only two choices:

```txt
Take it
Don't take it
```

This immediately suggests:

```txt
Pick / Not Pick DP
```

---

# State

```cpp
dp[i][k]
```

=

```txt
Can we form sum k
using elements from index 0...i ?
```

---

# Choices

For element:

```cpp
arr[i]
```

### Not Take

```cpp
dp[i-1][k]
```

---

### Take

Possible only if:

```cpp
arr[i] <= k
```

Then:

```cpp
dp[i-1][k-arr[i]]
```

---

# Recurrence

```cpp
dp[i][k]
=
dp[i-1][k]
||
dp[i-1][k-arr[i]]
```

Meaning:

```txt
Current target can be formed if:

1. We ignore current element

OR

2. We include current element
```

---

# Base Cases

## Sum = 0

```cpp
dp[i][0] = true
```

for all `i`.

Reason:

```txt
Empty subset always forms sum 0.
```

---

## First Element

```cpp
dp[0][arr[0]] = true
```

if:

```cpp
arr[0] <= target
```

Reason:

```txt
Using only the first element,
we can form its value.
```

---

# Memoization

## State

```cpp
find(ind, target)
```

=

```txt
Can we form target
using elements 0...ind ?
```

---

## Recurrence

```cpp
notTake = find(ind-1, target)

take =
find(ind-1, target-arr[ind])
```

if possible.

---

## Complexity

| Metric | Complexity                     |
| ------ | ------------------------------ |
| Time   | O(n × target)                  |
| Space  | O(n × target) + O(n) recursion |

---

# Tabulation

## State

```cpp
dp[i][k]
```

same meaning.

---

## Fill Order

```txt
Top → Bottom

Target:
0 → target
```

because:

```cpp
dp[i][k]
```

depends on:

```cpp
dp[i-1][...]
```

---

## Complexity

| Metric | Complexity    |
| ------ | ------------- |
| Time   | O(n × target) |
| Space  | O(n × target) |

---

# Space Optimization

## Observation

Current row depends only on:

```txt
Previous row
```

Specifically:

```cpp
dp[i-1][k]

dp[i-1][k-arr[i]]
```

---

Therefore we only need:

```cpp
prev[]
curr[]
```

instead of the entire table.

---

## Optimized State

```cpp
prev[k]
```

=

```txt
Can we form sum k
using processed elements?
```

---

## Transition

```cpp
notTake = prev[k]

take = prev[k-arr[i]]
```

if possible.

---

```cpp
curr[k] = take || notTake;
```

---

## Complexity

| Metric | Complexity    |
| ------ | ------------- |
| Time   | O(n × target) |
| Space  | O(target)     |

---

# Pattern Recognition

```txt
Category:
Pick / Not Pick DP
```

State:

```txt
(index, target)
```

Choices:

```txt
Take
Don't Take
```

Combine:

```txt
OR
```

because we only need existence.

---

# General Template

Whenever you see:

```txt
Can we make target X?

Can we achieve sum X?

Can we choose some elements
to get X?
```

Think:

```txt
State:
(index, target)

Choices:
Take / Don't Take
```

---

# Relation to Future Problems

This exact framework is used in:

### Partition Equal Subset Sum

```txt
Same DP
Different target
```

---

### Count Subsets with Sum K

```txt
Replace OR with +
```

Count ways instead of existence.

---

### Target Sum

```txt
Transform into subset sum
```

---

### 0/1 Knapsack

```txt
Take / Don't Take
```

but maximize value.

---

### Minimum Subset Sum Difference

```txt
Use reachable subset sums
```

from this DP table.

---

# Key Takeaway

```txt
If the problem asks:

"Can we form target X
using some elements?"

the state is usually:

(index, target)

and the recurrence is:

Take
or
Don't Take.
```

This is one of the most important DP patterns for interviews and placements.
