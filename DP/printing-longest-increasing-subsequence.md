# Print Longest Increasing Subsequence (LIS)

## References

**Question:**

[https://www.geeksforgeeks.org/problems/printing-longest-increasing-subsequence/1](https://www.geeksforgeeks.org/problems/printing-longest-increasing-subsequence/1)

**Related Problems:**

- Longest Increasing Subsequence
- Number of Longest Increasing Subsequences
- Largest Divisible Subset
- Longest Bitonic Subsequence

---

# Pattern

```text
Subsequence DP

Parent Tracking

Path Reconstruction

Patience Sorting + Binary Search
```

---

# Problem Statement

Given an array, return **one** Longest Increasing Subsequence.

Unlike the normal LIS problem, here we must reconstruct the sequence.

---

# Approach 1 : O(n²) DP + Parent Array

## DP State

```cpp
dp[i]
```

Meaning

```text
Length of LIS

ending at index i.
```

---

## Additional Array

```cpp
parent[i]
```

Meaning

```text
Previous index

used to build the LIS

ending at i.
```

Initially

```cpp
parent[i] = i;
```

---

## Transition

For every

```cpp
j < i
```

if

```cpp
nums[j] < nums[i]
```

and

```cpp
dp[j] + 1 > dp[i]
```

then

```cpp
dp[i] = dp[j] + 1;

parent[i] = j;
```

---

## Reconstructing the LIS

Find the index having maximum

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

Finally

```cpp
reverse(answer.begin(), answer.end());
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

# Approach 2 : O(n log n) Patience Sorting + Parent Array

## Key Observation

The patience sorting algorithm only computes the **length**.

To reconstruct the sequence, additional information must be stored.

---

## Data Structures

### tailIndex

```cpp
vector<int> tailIndex;
```

Meaning

```text
tailIndex[len]

=

Index of the smallest possible tail

of an LIS

having length (len + 1).
```

Notice

```text
It stores INDICES,

not values.
```

---

### parent

```cpp
vector<int> parent(n, -1);
```

Meaning

```text
parent[i]

=

Previous index

in the LIS ending at i.
```

---

# Finding the Position

For every element

```cpp
nums[i]
```

find the first tail

```text
>= nums[i]
```

using

```cpp
int pos = lower_bound(
    tailIndex.begin(),
    tailIndex.end(),
    nums[i],
    [&](int idx, int value){
        return nums[idx] < value;
    }
) - tailIndex.begin();
```

Here

```text
pos
```

represents

```text
The LIS length

that nums[i]

will belong to.
```

---

# Parent Update

If

```cpp
pos > 0
```

then the predecessor of

```cpp
nums[i]
```

must be

```cpp
tailIndex[pos-1]
```

Therefore

```cpp
parent[i] = tailIndex[pos-1];
```

If

```cpp
pos == 0
```

then

```cpp
parent[i] = -1;
```

---

# Updating the Tail

If

```cpp
pos == tailIndex.size()
```

the current element extends the longest LIS.

```cpp
tailIndex.push_back(i);
```

Otherwise

```cpp
tailIndex[pos] = i;
```

Replace the tail with a better (smaller) ending element.

---

# Reconstructing the LIS

The last element of the LIS is

```cpp
tailIndex.back()
```

Follow parent pointers.

```cpp
curr = tailIndex.back();

while(curr != -1){

    answer.push_back(nums[curr]);

    curr = parent[curr];
}
```

Finally

```cpp
reverse(answer.begin(), answer.end());
```

---

# Why Does This Work?

The invariant maintained is

```text
tailIndex[len]

↓

Index of the smallest possible tail

for every LIS length.
```

A smaller tail is always better because it can be extended by more future elements.

---

# Important Insight

The vector

```cpp
tailIndex
```

does **NOT** store the actual LIS.

It only stores

```text
Best possible tail

for every length.
```

The actual sequence is reconstructed entirely using

```cpp
parent[]
```

---

# Common Mistake #1

Using

```cpp
vector<pair<int,int>>
```

to store

```text
(value,index)
```

Not required.

Storing only indices is sufficient.

---

# Common Mistake #2

Writing

```cpp
*lower_bound(...)
```

This returns the stored value (index), **not** the position.

Correct

```cpp
lower_bound(...) - tailIndex.begin()
```

---

# Common Mistake #3

Writing

```cpp
parent[pos]
```

The parent belongs to the **current element**, not the position.

Correct

```cpp
parent[i]
```

---

# Common Mistake #4

Writing

```cpp
parent[i] = parent[tailIndex[pos]];
```

Incorrect.

The predecessor of the current element is

```cpp
tailIndex[pos-1]
```

not the predecessor of the replaced tail.

Correct

```cpp
parent[i] = tailIndex[pos-1];
```

---

# Common Mistake #5

Thinking

```cpp
tailIndex
```

stores the LIS.

It does not.

It stores only the **best tail** for each LIS length.

---

# Complexity

## O(n²) DP + Parent

Time

```text
O(n²)
```

Space

```text
O(n)
```

---

## O(n log n) Patience Sorting + Parent

Time

```text
O(n log n)
```

Space

```text
O(n)
```

---

# Pattern Recognition

Whenever the problem asks

```text
Print

Longest Increasing Subsequence
```

think

```text
Length only

↓

Patience Sorting

+

Need actual sequence

↓

Store Indices

+

Parent Array

↓

Reconstruct by following parents
```

---

# Interview Trigger

```text
Print LIS

↓

Need Reconstruction

↓

Parent Array

↓

Patience Sorting

↓

tailIndex

=

Index of smallest tail

↓

Binary Search

↓

Find Position

↓

parent[i] = tailIndex[pos-1]

↓

Update tailIndex

↓

Start from tailIndex.back()

↓

Follow parents

↓

Reverse

↓

O(n log n)
```

---

# Core Insight

The **tail array** is optimized for computing the **length** of the LIS, not for storing the sequence itself. To reconstruct the actual LIS while preserving the **O(n log n)** complexity, maintain:

- **`tailIndex`**: the index of the smallest possible tail for every LIS length.
- **`parent`**: the predecessor of each element in the reconstructed subsequence.

The crucial observation is:

```cpp
parent[i] = tailIndex[pos - 1];
```

because if `nums[i]` becomes the tail of an LIS of length `L`, then its predecessor must be the current tail of an LIS of length `L - 1`. This single relationship makes efficient reconstruction possible.
