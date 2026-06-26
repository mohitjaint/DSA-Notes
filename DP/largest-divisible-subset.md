# Largest Divisible Subset

## References

**Question:**

[https://leetcode.com/problems/largest-divisible-subset/](https://leetcode.com/problems/largest-divisible-subset/)

**Related Problems:**

- Longest Increasing Subsequence
- Print Longest Increasing Subsequence
- Longest String Chain
- Largest Divisible Pair Chain

---

# Pattern

```text
Subsequence DP

LIS Variation

Parent Tracking

Sequence Reconstruction
```

---

# Problem Statement

Given a set of distinct positive integers, return the **largest subset** such that

```text
For every pair

(a, b)

either

a divides b

OR

b divides a
```

If multiple answers exist, return any one.

---

# Key Observation

Sort the array first.

```cpp
sort(nums.begin(), nums.end());
```

After sorting,

```text
a ≤ b ≤ c
```

If

```text
b % a == 0

and

c % b == 0
```

then

```text
c % a == 0
```

because

```text
c = k₁ × b

b = k₂ × a

⇒

c = (k₁ × k₂) × a
```

Thus, divisibility becomes **transitive**, allowing us to build the answer exactly like LIS.

---

# DP State

```cpp
dp[i]
```

Meaning

```text
Length of the Largest Divisible Subset

ending at index i.
```

---

# Parent Array

```cpp
parent[i]
```

Meaning

```text
Previous index

used to build the subset

ending at i.
```

Initially

```cpp
parent[i] = i;
```

---

# Initialization

Every element alone forms a valid subset.

```cpp
dp[i] = 1;
```

---

# Transition

For every

```cpp
j < i
```

if

```cpp
nums[i] % nums[j] == 0
```

then

```cpp
if(dp[j] + 1 > dp[i]){

    dp[i] = dp[j] + 1;

    parent[i] = j;
}
```

---

# Reconstructing the Answer

Find the index having the maximum

```cpp
dp[i]
```

Suppose it is

```cpp
lastIndex
```

Follow

```cpp
parent[lastIndex]
```

until

```cpp
parent[index] == index
```

Push every element into the answer.

Finally

```cpp
reverse(answer.begin(), answer.end());
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

LIS transition

```cpp
if(nums[j] < nums[i])
```

Largest Divisible Subset transition

```cpp
if(nums[i] % nums[j] == 0)
```

Everything else remains identical.

| LIS                         | Largest Divisible Subset                         |
| --------------------------- | ------------------------------------------------ |
| `dp[i]` = LIS ending at `i` | `dp[i]` = Largest Divisible Subset ending at `i` |
| `parent[i]`                 | `parent[i]`                                      |
| Reconstruct using parent    | Reconstruct using parent                         |

---

# Why Sorting is Necessary

Without sorting,

```text
8 2 4
```

cannot be processed correctly because the divisor may appear after its multiple.

After sorting,

```text
2 4 8
```

every valid predecessor appears before the current element.

---

# Can We Use the O(n log n) LIS Optimization?

**No.**

The binary search optimization for LIS depends on maintaining

```text
Smallest possible tail

for every subsequence length.
```

A smaller tail is always better because it can be extended by every element that extends a larger tail.

Example

```text
Tail = 3

is always better than

Tail = 5
```

---

For divisibility, this property does **not** hold.

Example

```text
Tail = 6

Tail = 8
```

Future element

```text
16
```

extends only

```text
8
```

Future element

```text
18
```

extends only

```text
6
```

Neither tail dominates the other.

Therefore there is **no unique "best tail"** to maintain.

---

# Why Binary Search Fails

Binary search requires a **monotonic** property.

For LIS

```text
2 < 5 < 7 < 18
```

The tails are sorted.

Hence

```cpp
lower_bound()
```

works.

For divisibility,

valid predecessors are scattered.

Example

```text
24
```

Valid predecessors

```text
1

2

3

4

6

8

12
```

They do **not** form a contiguous range.

Therefore binary search cannot identify them.

---

# Common Mistake #1

Forgetting to sort.

Without sorting,

the DP transition is invalid.

---

# Common Mistake #2

Using

```cpp
nums[j] % nums[i] == 0
```

instead of

```cpp
nums[i] % nums[j] == 0
```

Since

```text
j < i
```

the previous element must divide the current one.

---

# Common Mistake #3

Forgetting

```cpp
parent[i] = i;
```

during initialization.

Without this, reconstruction may fail.

---

# Common Mistake #4

Not reversing the reconstructed answer.

The parent chain reconstructs the subset from

```text
Last

↓

First
```

Reverse before returning if increasing order is desired.

---

# Pattern Recognition

Whenever a problem asks

```text
Longest Chain

Largest Chain

Longest Subsequence
```

ask yourself

```text
Can I sort first?

↓

Can I define

dp[i]

=

Best answer ending at i?

↓

Can I reconstruct using a parent array?
```

If yes, it is often an **LIS-style DP**.

---

# Interview Trigger

```text
Largest Divisible Subset

↓

Sort Array

↓

dp[i]

=

Largest subset ending at i

↓

Transition

nums[i] % nums[j] == 0

↓

Parent Array

↓

Reconstruct Answer

↓

O(n²)
```

---

# Core Insight

This problem is a direct variation of **Longest Increasing Subsequence**. The only change is the condition for extending a subsequence:

- **LIS:** `nums[j] < nums[i]`
- **Largest Divisible Subset:** `nums[i] % nums[j] == 0`

The famous **O(n log n)** LIS optimization does **not** apply because divisibility does not provide a monotonic "best tail" that can safely replace another. Therefore, the optimal standard solution remains the **O(n²)** DP with parent reconstruction.
