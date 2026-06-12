# Count Subsets with Sum K (Perfect Sum)

## References

Problem Link:
https://www.geeksforgeeks.org/problems/perfect-sum-problem5633/1

Related Problems:

Subset Sum:
https://www.geeksforgeeks.org/problems/subset-sum-problem-1611555638/1

Target Sum (LC 494):
https://leetcode.com/problems/target-sum/

Partition Equal Subset Sum (LC 416):
https://leetcode.com/problems/partition-equal-subset-sum/

Striver DP Playlist:
https://takeuforward.org/dynamic-programming/

---

# Pattern

```txt
DP on Sum

Count Number of Ways

0/1 Knapsack Variant
```

---

# Key Observation

Classic Subset Sum asks:

```txt
Can we make target K?
```

This problem asks:

```txt
How many subsets make target K?
```

So instead of storing:

```cpp
true / false
```

store:

```cpp
number of ways
```

---

# State Definition

```cpp
dp[i][target]
```

Meaning:

```txt
Number of subsets from indices [0...i]
whose sum equals target.
```

---

# Transition

For every element:

```cpp
arr[i]
```

We have two choices:

---

## Not Take

```cpp
dp[i-1][target]
```

---

## Take

Possible only if:

```cpp
arr[i] <= target
```

Contribution:

```cpp
dp[i-1][target-arr[i]]
```

---

## Final Transition

```cpp
dp[i][target]
=
dp[i-1][target]
+
dp[i-1][target-arr[i]]
```

or

```cpp
dp[i][target]
=
(notTake + take) % MOD
```

---

# Why It Works

Every valid subset:

```txt
Either contains arr[i]
Or does not contain arr[i]
```

These two cases are disjoint.

Therefore:

```txt
Total Ways
=
Ways(Not Take)
+
Ways(Take)
```

---

# Base Cases

Sum:

```cpp
target = 0
```

can always be formed by:

```txt
Empty Subset
```

Therefore:

```cpp
dp[i][0] = 1
```

for all valid indices.

---

For first element:

```cpp
if(arr[0] <= K)
    dp[0][arr[0]] = 1;
```

Meaning:

```txt
Subset = {arr[0]}
```

forms that sum.

---

# Implementation

```cpp
for(int i = 1; i < n; i++){

    for(int target = 1; target <= K; target++){

        if(arr[i] <= target){

            dp[i][target] =
            (
                dp[i-1][target]
                +
                dp[i-1][target-arr[i]]
            ) % MOD;

        }
        else{

            dp[i][target]
            =
            dp[i-1][target];
        }
    }
}
```

---

# Complexity

## Time

States:

```txt
n × K
```

Transition:

```txt
O(1)
```

Total:

```txt
O(n × K)
```

---

## Space

```txt
O(n × K)
```

---

# Space Optimization

Observation:

```txt
Current row depends only on previous row.
```

Therefore:

```txt
O(K)
```

space is possible.

---

## Optimized DP

```cpp
vector<int> dp(K+1,0);

dp[0] = 1;

for(int num : arr){

    for(int target = K;
        target >= num;
        target--){

        dp[target]
        =
        (dp[target]
         + dp[target-num]) % MOD;
    }
}
```

---

# Common Mistakes

### ❌ Using Boolean DP

Wrong:

```cpp
dp[i][target] = true/false
```

Need:

```cpp
dp[i][target] = count
```

---

### ❌ Forget Modulo

```cpp
MOD = 1e9+7
```

Answer can become very large.

Use:

```cpp
% MOD
```

during every update.

---

### ❌ Forward Traversal in 1D DP

Wrong:

```cpp
for(target = num; target <= K; target++)
```

This converts it into:

```txt
Unbounded Knapsack
```

Use:

```cpp
for(target = K; target >= num; target--)
```

for 0/1 selection.

---

### ❌ Confusing With Subset Sum

Subset Sum:

```txt
Can we make K?
```

Stores:

```cpp
bool
```

Perfect Sum:

```txt
How many ways to make K?
```

Stores:

```cpp
int
```

---

# Pattern Recognition

Whenever you see:

```txt
Count subsets

Count partitions

Number of ways

Count combinations

Target Sum
```

Think:

```txt
Boolean DP
↓
Replace OR with +
↓
Store Counts
```

---

# Related Problems

### Prerequisites

- Subset Sum Problem
- 0/1 Knapsack Problem

### Similar

- Target Sum
- Partitions With Given Difference
- Partition Equal Subset Sum

---

# Constraint Analysis

```txt
n ≤ 100
K ≤ 1000
```

Notice:

```txt
n is large
K is small
```

Therefore:

```txt
DP on Sum ✓
Meet in the Middle ✗
```

Reason:

```txt
MITM
→ O(2^(n/2))
→ O(2^50)

Impossible
```

while:

```txt
DP
→ O(n × K)
→ 100 × 1000
→ 10^5
```

which is easily feasible.
