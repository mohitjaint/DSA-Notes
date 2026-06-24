# Longest Increasing Subsequence (LIS)

## References

**Question:**

[https://leetcode.com/problems/longest-increasing-subsequence/](https://leetcode.com/problems/longest-increasing-subsequence/)

**Related Problems:**

- Print Longest Increasing Subsequence
- Number of Longest Increasing Subsequences
- Longest Bitonic Subsequence
- Russian Doll Envelopes
- Largest Divisible Subset

---

# Pattern

```text
Subsequence DP

Take / Not Take DP

State Redesign

1D DP
```

---

# Problem Statement

Given an integer array, return the length of the **Longest Increasing Subsequence (LIS)**.

A subsequence is formed by deleting zero or more elements without changing the relative order of the remaining elements.

---

# Approach 1 : Memoization

## DP State

```cpp
solve(i, prev)
```

Meaning

```text
Maximum LIS

starting from index i

when the previously selected element
is at index prev.
```

---

## Base Case

```cpp
if(i == n)
    return 0;
```

---

## Choices

### Skip Current Element

```cpp
solve(i+1, prev)
```

---

### Take Current Element

Allowed only if

```cpp
prev == -1

OR

nums[i] > nums[prev]
```

Transition

```cpp
1 + solve(i+1, i)
```

---

## Recurrence

```cpp
int notTake = solve(i+1, prev);

int take = 0;

if(prev == -1 || nums[i] > nums[prev]){
    take = 1 + solve(i+1, i);
}

return max(take, notTake);
```

---

## Memoization Table

Since

```text
prev ∈ {-1, 0, 1, ..., n-1}
```

store

```text
prev + 1
```

State

```cpp
dp[i][prev+1]
```

---

## Complexity

Time

```text
O(n²)
```

Space

```text
O(n²)
```

---

# Approach 2 : Tabulation

## State

Same as memoization

```cpp
dp[i][prev+1]
```

---

## Iteration Order

Since every state depends on

```text
i + 1
```

iterate

```cpp
for(int i = n-1; i >= 0; i--)
```

---

## Previous Index Loop

Important observation

For state

```text
(i, prev)
```

the previous element must always occur before the current element.

Therefore

```text
prev < i
```

Loop

```cpp
for(int prev = i-1; prev >= -1; prev--)
```

---

## Transition

```cpp
int notTake = dp[i+1][prev+1];

int take = 0;

if(prev == -1 || nums[i] > nums[prev]){
    take = 1 + dp[i+1][i+1];
}

dp[i][prev+1] = max(take, notTake);
```

---

## Answer

```cpp
dp[0][0]
```

---

## Complexity

Time

```text
O(n²)
```

Space

```text
O(n²)
```

---

# Approach 3 : Space Optimization

## Observation

Current row depends only on

```text
Next Row
```

Maintain

```cpp
ahead

curr
```

instead of the complete DP table.

---

## Transition

```cpp
int notTake = ahead[prev+1];

int take = 0;

if(prev == -1 || nums[i] > nums[prev]){
    take = 1 + ahead[i+1];
}

curr[prev+1] = max(take, notTake);
```

After each row

```cpp
ahead = curr;
```

---

## Complexity

Time

```text
O(n²)
```

Space

```text
O(n)
```

---

# Approach 4 : State Redesign (1D DP)

## Key Observation

Instead of asking

```text
What is the LIS

starting from i?
```

ask

```text
What is the LIS

ending at i?
```

This removes the need to store the previous index.

---

## DP State

```cpp
dp[i]
```

Meaning

```text
Length of the Longest Increasing Subsequence

ending at index i.
```

---

## Initialization

Every element itself is an increasing subsequence.

```cpp
dp[i] = 1;
```

---

## Transition

For every previous index

```cpp
j < i
```

if

```cpp
nums[j] < nums[i]
```

then

```cpp
dp[i] = max(dp[i], dp[j] + 1);
```

---

## Complete Transition

```cpp
for(int i = 0; i < n; i++){

    dp[i] = 1;

    for(int j = 0; j < i; j++){

        if(nums[j] < nums[i]){
            dp[i] = max(dp[i], dp[j] + 1);
        }
    }
}
```

---

## Answer

The LIS may end at **any** index.

Therefore

```cpp
*max_element(dp.begin(), dp.end())
```

---

## Why Not Return `dp[n-1]`?

Example

```text
1 2 3 4 0
```

DP

```text
1 2 3 4 1
```

The LIS ends at index `3`, not at the last index.

Hence

```cpp
return *max_element(dp.begin(), dp.end());
```

---

## Complexity

Time

```text
O(n²)
```

Space

```text
O(n)
```

---

# Common Mistake #1

Using

```cpp
for(int prev = i; ...)
```

The previous element cannot be the current element.

Correct

```cpp
for(int prev = i-1; prev >= -1; prev--)
```

---

# Common Mistake #2

Forgetting the shifted index

```cpp
dp[i][prev+1]
```

instead of

```cpp
dp[i][prev]
```

---

# Common Mistake #3

Returning

```cpp
dp[n-1]
```

in the 1D DP solution.

The LIS can end anywhere.

Return

```cpp
*max_element(dp.begin(), dp.end());
```

---

# Pattern Recognition

Whenever the problem asks

```text
Longest

Increasing

Subsequence
```

think

```text
Can I define

dp[i]

=

Best answer

ending at i?
```

This removes one DP dimension and leads to the elegant **O(n²)** solution.

---

# Interview Trigger

```text
LIS

↓

Take / Not Take

↓

State

(i, prev)

↓

Memoization

↓

Tabulation

↓

Space Optimization

↓

Observe Better State

↓

dp[i]

=

LIS ending at i

↓

Transition

dp[i] = max(dp[j] + 1)

↓

Answer

max(dp)

↓

O(n²)
```

---

# Core Insight

The biggest improvement in LIS is **not** space optimization.

It is **state redesign**.

Instead of tracking both the current index and the previous chosen index, redefine the state as:

```text
dp[i]

=

Length of the LIS ending at index i.
```

This removes an entire DP dimension while preserving the same time complexity, making the solution significantly simpler and serving as a classic example of choosing a better DP state.
