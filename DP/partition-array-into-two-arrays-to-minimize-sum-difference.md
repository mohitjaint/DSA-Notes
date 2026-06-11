# 2035. Partition Array Into Two Arrays to Minimize Sum Difference

## References

- [LeetCode: Partition Array Into Two Arrays to Minimize Sum Difference](https://leetcode.com/problems/partition-array-into-two-arrays-to-minimize-sum-difference/description/)

---

## Pattern

```txt
Meet in the Middle

Exact subset selection

n = 30

Minimize absolute difference
```

---

# Key Observation

We must select:

```txt
Exactly n elements
```

from:

```txt
2n elements
```

Let:

```cpp
selectedSum = sum of chosen n elements
```

Then answer becomes:

```txt
|totalSum - 2*selectedSum|
```

because:

```txt
Group1 = selectedSum

Group2 = totalSum - selectedSum
```

Difference:

```txt
|Group1 - Group2|

= |selectedSum - (totalSum - selectedSum)|

= |totalSum - 2*selectedSum|
```

Goal:

```txt
selectedSum ≈ totalSum/2
```

---

# Why Meet in the Middle?

Constraints:

```txt
N = 30
```

Bruteforce:

```txt
2^30 ≈ 1 billion subsets
```

Too large.

Split array into:

```txt
Left Half  = first n elements
Right Half = last n elements
```

Now:

```txt
2^15 = 32768
```

which is manageable.

---

# Step 1: Generate Subset Sums

For every subset of both halves:

```cpp
mask = 0 → (1<<n)-1
```

Calculate:

```cpp
subset size
left sum
right sum
```

Store:

```cpp
left[k]
right[k]
```

where:

```txt
k = number of selected elements
```

---

## Example

```txt
nums = [1,2,3,4]
```

Split:

```txt
Left  = [1,2]
Right = [3,4]
```

Possible storage:

```txt
left[0]  = {0}
left[1]  = {1,2}
left[2]  = {3}

right[0] = {0}
right[1] = {3,4}
right[2] = {7}
```

---

# Step 2: Fix Number of Chosen Elements

Need exactly:

```txt
n elements
```

If we choose:

```txt
i elements from left
```

then we must choose:

```txt
n-i elements from right
```

Therefore:

```cpp
left[i]
```

must combine with:

```cpp
right[n-i]
```

---

# Step 3: Binary Search Best Complement

Suppose:

```cpp
a = left sum
b = right sum
```

Selected sum:

```cpp
a + b
```

Need:

```txt
a + b ≈ totalSum/2
```

Therefore:

```cpp
b ≈ totalSum/2 - a
```

Target:

```cpp
target = totalSum/2 - a
```

Search inside:

```cpp
right[n-i]
```

using:

```cpp
lower_bound()
```

---

# Important Binary Search Trick

After:

```cpp
auto itr = lower_bound(...)
```

Always check:

```cpp
itr
```

and

```cpp
itr-1
```

because nearest value can lie on either side.

---

## Example

```txt
target = 10

values = [4,8,12,20]
```

Lower bound:

```txt
12
```

But:

```txt
8
```

may produce a better answer.

Check both.

---

# Step 4: Update Answer

Candidate sum:

```cpp
selectedSum = a + b
```

Difference:

```cpp
abs(totalSum - 2*selectedSum)
```

Update:

```cpp
ans = min(ans,
          abs(totalSum - 2*(a+b)));
```

---

# Complexity

## Generate Subsets

```txt
2^n masks
```

For each:

```txt
O(n)
```

Total:

```txt
O(n * 2^n)
```

---

## Sorting

```txt
O(2^n log(2^n))
```

---

## Binary Search

For every subset sum:

```txt
O(2^n log(2^n))
```

---

## Total

```txt
O(n·2^n)
```

For n = 15:

```txt
≈ 5 × 10^5 operations
```

Very manageable.

---

# Common Mistakes

### ❌ Forget grouping by subset size

Wrong:

```cpp
allLeftSums
allRightSums
```

Need:

```cpp
left[k]
right[k]
```

because exactly:

```txt
n elements
```

must be chosen.

---

### ❌ Search only lower_bound result

Wrong:

```cpp
check(*itr)
```

Correct:

```cpp
check(*itr)

check(*(itr-1))
```

---

### ❌ Use

```cpp
for(i = 1; i < n; i++)
```

Better:

```cpp
for(i = 0; i <= n; i++)
```

Then edge cases:

```txt
0 from left + n from right
n from left + 0 from right
```

are handled automatically.

---

### ❌ Forget answer formula

Wrong:

```cpp
ans = min(ans, subsetSum);
```

Correct:

```cpp
ans = min(ans,
          abs(totalSum - 2*subsetSum));
```

---

### ❌ Ignore overflow

Prefer:

```cpp
long long
```

for:

```cpp
sum
subset sums
answer
```

---

# Pattern Recognition

Whenever you see:

```txt
n ≈ 30

Subset Selection

Exact element count

Subset Sum Optimization
```

Think:

```txt
Meet in the Middle
```

Workflow:

```txt
Split Array
↓
Generate Subset Sums
↓
Group by Size
↓
Sort One Side
↓
Binary Search Complement
```

---

# Template

```txt
n ≤ 40
↓
2^n too large
↓
Split into two halves
↓
Generate all subset sums
↓
Sort one half
↓
Binary search complement
```

This is one of the most important **Meet-in-the-Middle** patterns asked in interviews and advanced DP/optimization problems.
