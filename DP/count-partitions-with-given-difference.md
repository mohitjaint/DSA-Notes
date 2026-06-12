# Partitions With Given Difference

## References

Question:

[https://www.geeksforgeeks.org/problems/partitions-with-given-difference/1](https://www.geeksforgeeks.org/problems/partitions-with-given-difference/1)

Related Problems:

[https://www.geeksforgeeks.org/problems/subset-sum-problem-1611555638/1](https://www.geeksforgeeks.org/problems/subset-sum-problem-1611555638/1)

[https://www.geeksforgeeks.org/problems/perfect-sum-problem5633/1](https://www.geeksforgeeks.org/problems/perfect-sum-problem5633/1)

[https://leetcode.com/problems/target-sum/](https://leetcode.com/problems/target-sum/)

[https://leetcode.com/problems/partition-equal-subset-sum/](https://leetcode.com/problems/partition-equal-subset-sum/)

---

# Pattern

```txt
DP on Sum

Count Number of Ways

0/1 Knapsack Counting Variant

Reduction to Count Subsets With Sum K
```

---

# Key Observation

We need:

```txt
S1 - S2 = diff
```

and

```txt
S1 + S2 = totalSum
```

Adding both equations:

```txt
2*S1 = totalSum + diff
```

Subtracting:

```txt
2*S2 = totalSum - diff
```

We can count either:

```txt
S1 = (totalSum + diff)/2
```

or

```txt
S2 = (totalSum - diff)/2
```

Most solutions use:

```txt
target = (totalSum - diff)/2
```

Now the problem becomes:

```txt
Count subsets having sum = target
```

which is exactly the Perfect Sum problem.

---

# Constraint Analysis

```txt
n ≤ 100

arr[i] ≤ 50

totalSum ≤ 5000
```

Notice:

```txt
n is large
target/sum is small
```

Therefore:

```txt
DP on Sum ✓

Meet in the Middle ✗
```

Reason:

```txt
MITM

2^(n/2)

2^50

Impossible
```

while:

```txt
O(n × target)
```

is feasible.

---

# Invalid Cases

Before building DP:

```cpp
if(totalSum - diff < 0)
    return 0;
```

Reason:

```txt
Subset sum cannot be negative.
```

---

Also:

```cpp
if((totalSum - diff) % 2)
    return 0;
```

Reason:

```txt
target must be an integer.
```

---

# Bruteforce

For every element:

```txt
Take
Not Take
```

Total states:

```txt
2^n
```

Complexity:

```txt
O(2^n)
```

Too large for:

```txt
n = 100
```

---

# Optimized Solution

## Reduction

Compute:

```cpp
target = (totalSum - diff)/2;
```

Now count subsets whose sum equals:

```cpp
target
```

---

## State

```cpp
dp[i][target]
```

Meaning:

```txt
Number of subsets from [0...i]
having sum = target
```

---

## Transition

### Not Take

```cpp
dp[i-1][target]
```

---

### Take

Possible only if:

```cpp
arr[i] <= target
```

Contribution:

```cpp
dp[i-1][target-arr[i]]
```

---

### Final Transition

```cpp
dp[i][target]
=
dp[i-1][target]
+
dp[i-1][target-arr[i]]
```

---

# Base Case

If first element is:

```cpp
0
```

Then:

```txt
Take 0
Not Take 0
```

both produce sum:

```txt
0
```

Therefore:

```cpp
dp[0][0] = 2;
```

---

Otherwise:

```cpp
dp[0][0] = 1;
```

and

```cpp
dp[0][arr[0]] = 1;
```

if valid.

---

# Space Optimization

Observation:

```txt
Current row depends only on previous row.
```

Therefore:

```txt
O(target)
```

space is possible.

---

## 1D DP

```cpp
dp[target]
```

Meaning:

```txt
Number of ways to form target
using processed elements.
```

Transition:

```cpp
dp[target]
=
dp[target]
+
dp[target-arr[i]]
```

---

# Why Backward Iteration?

Need:

```cpp
dp[target-arr[i]]
```

from the:

```txt
Previous Row
```

If we iterate forward:

```cpp
0 → target
```

then:

```cpp
dp[target-arr[i]]
```

may already be updated.

This reuses the same element multiple times.

Problem becomes:

```txt
Unbounded Knapsack
```

which is wrong.

---

Use:

```cpp
target = K → 0
```

so:

```cpp
dp[target-arr[i]]
```

still contains the old value.

---

# Why It Works

Every subset:

```txt
Either takes arr[i]
Or does not take arr[i]
```

These cases are disjoint.

Therefore:

```txt
Total Ways

=
Take Ways
+
Not Take Ways
```

which gives the recurrence.

---

# Complexity

## Tabulation

Time:

```txt
O(n × target)
```

Space:

```txt
O(n × target)
```

---

## Space Optimized

Time:

```txt
O(n × target)
```

Space:

```txt
O(target)
```

---

# Common Mistakes

### ❌ Forget Reduction

Wrong:

```txt
Directly partition arrays.
```

Think:

```txt
Convert to Count Subsets With Sum K
```

first.

---

### ❌ Forget Validity Checks

```cpp
(totalSum - diff) < 0
```

or

```cpp
(totalSum - diff) % 2 != 0
```

must return:

```cpp
0
```

---

### ❌ Iterate Forward in 1D DP

Wrong:

```cpp
for(target = 0; target <= K; target++)
```

This allows reusing the same element.

Use:

```cpp
for(target = K; target >= 0; target--)
```

---

### ❌ Incorrect Handling of Zero

For:

```cpp
arr[i] = 0
```

both:

```txt
Take
Not Take
```

produce same sum.

Need:

```cpp
dp[0][0] = 2
```

for the first element.

---

# Pattern Recognition

Whenever you see:

```txt
Count Partitions

Difference = D

Number of Ways

Assign + and - signs
```

Think:

```txt
S1 - S2 = D
S1 + S2 = Total
```

↓

```txt
Subset Sum = (Total - D)/2
```

↓

```txt
Count Subsets With Sum K
```

---

# Related Problems

### Prerequisites

[https://www.geeksforgeeks.org/problems/subset-sum-problem-1611555638/1](https://www.geeksforgeeks.org/problems/subset-sum-problem-1611555638/1)

[https://www.geeksforgeeks.org/problems/perfect-sum-problem5633/1](https://www.geeksforgeeks.org/problems/perfect-sum-problem5633/1)

---

### Same Pattern

[https://leetcode.com/problems/target-sum/](https://leetcode.com/problems/target-sum/)

[https://leetcode.com/problems/partition-equal-subset-sum/](https://leetcode.com/problems/partition-equal-subset-sum/)

---

### Advanced

[https://leetcode.com/problems/partition-array-into-two-arrays-to-minimize-sum-difference/](https://leetcode.com/problems/partition-array-into-two-arrays-to-minimize-sum-difference/)

---

# Interview Trigger

```txt
If you see:

Count partitions

Difference = D

Assign + and - signs

Count ways

Think:

S1 - S2 = D
↓
Subset Sum Reduction
↓
Count Subsets With Sum K
↓
0/1 Knapsack Counting DP
```
