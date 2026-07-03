# Max Consecutive Ones III

## References

**Question:**

[https://leetcode.com/problems/max-consecutive-ones-iii/](https://leetcode.com/problems/max-consecutive-ones-iii/)

**Related Problems:**

- Longest Repeating Character Replacement
- Fruit Into Baskets
- Longest Substring Without Repeating Characters
- Binary Subarrays With Sum

---

# Pattern

```text
Sliding Window

Variable Size Window

Two Pointers
```

---

# Problem Statement

Given a binary array,

flip at most

```text
k
```

zeros into ones.

Return the length of the longest contiguous subarray containing only ones after the flips.

---

# Brute Force

For every starting index,

expand the subarray until more than

```text
k
```

zeros are encountered.

Keep track of

- current zero count
- maximum length

---

## Complexity

```text
Time  : O(n²)

Space : O(1)
```

---

# Observation

The subarray is valid if

```text
Number of zeros ≤ k
```

As soon as

```text
zeros > k
```

the window becomes invalid.

Instead of restarting,

move the left pointer until the window becomes valid again.

---

# Sliding Window State

Maintain

```cpp
l
```

Left pointer

```cpp
r
```

Right pointer

```cpp
zeroes
```

Number of zeros inside the current window.

---

# Window Invariant

```text
Window

[l........r]

contains at most

k

zeros.
```

---

# Expand Window

Move

```cpp
r
```

one step.

If

```cpp
nums[r] == 0
```

increment

```cpp
zeroes
```

---

# Shrink Window

When

```text
zeroes > k
```

remove elements from the left.

```cpp
if(nums[l] == 0)
    zeroes--;

l++;
```

---

# Update Answer

Whenever the window is valid,

```cpp
answer = max(answer, windowLength);
```

---

# Standard Sliding Window

```cpp
for(int r=0;r<n;r++){

    if(nums[r]==0)
        zeroes++;

    while(zeroes>k){

        if(nums[l]==0)
            zeroes--;

        l++;
    }

    ans=max(ans,r-l+1);
}
```

---

# Complexity

Every pointer moves at most

```text
n
```

times.

```text
Time : O(2n)

Space : O(1)
```

Although written as

```text
O(2n)
```

it is mathematically

```text
O(n)
```

since constants are ignored.

---

# Striver's Optimization

Observation:

Before adding

```cpp
nums[r]
```

the window is already valid.

Adding one element can increase the number of zeros by **at most one**.

Therefore,

```text
zeroes

can become

k+1

but never

k+2
```

So instead of

```cpp
while(zeroes>k)
```

we only need

```cpp
if(zeroes>k)
```

---

# Optimized Code

```cpp
for(int r=0;r<n;r++){

    if(nums[r]==0)
        zeroes++;

    if(zeroes>k){

        if(nums[l]==0)
            zeroes--;

        l++;
    }

    ans=max(ans,r-l+1);
}
```

---

# Why Does `if` Work?

At every iteration,

```text
Window before expansion

↓

Valid

↓

Add exactly one element

↓

Window can become invalid by only one zero

↓

Removing one element is sufficient to keep pace
```

The left pointer advances at most once per iteration of the right pointer.

---

# Complexity

```text
Right pointer : n moves

Left pointer  : at most n moves

Overall

Time : O(n)

Space : O(1)
```

Although both versions are asymptotically `O(n)`, the `if` version performs slightly fewer operations and is a useful optimization for this specific problem.

---

# Alternative Approach

Store the indices of all zeros.

Whenever more than

```text
k
```

zeros are inside the window,

move

```cpp
l
```

to just after the oldest zero.

```cpp
l = zeroIndices[zeroIndices.size()-k-1] + 1;
```

---

# Complexity

```text
Time : O(n)

Space : O(number of zeros)

Worst Case : O(n)
```

---

# Common Mistake #1

Updating

```cpp
l
```

to

```cpp
zeroIndices[size-k]
```

instead of

```cpp
zeroIndices[size-k-1]
```

You need to remove the **oldest** zero.

---

# Common Mistake #2

Forgetting

```cpp
+1
```

```cpp
l = zeroIndex + 1;
```

The oldest zero must be excluded from the new window.

---

# Common Mistake #3

Using

```cpp
while
```

thinking it is always required.

For this problem,

```cpp
if
```

also works because the violation can increase by only one.

---

# Common Mistake #4

Thinking

```text
O(2n)
```

is worse than

```text
O(n)
```

They are asymptotically identical.

The distinction is only in the implementation and constant factors.

---

# Pattern Recognition

Whenever a problem says

```text
Longest / Maximum Length

subject to

At most K
```

think

```text
Variable Size Sliding Window
```

Maintain a window satisfying the constraint.

Expand the right pointer.

Shrink the left pointer whenever the constraint is violated.

---

# Interview Trigger

```text
Longest Subarray

↓

At most K violations

↓

Sliding Window

↓

Maintain violation count

↓

Expand right

↓

Shrink left when invalid

↓

Update answer

↓

O(n)
```

---

# Core Insight

The validity of the window depends **only on the number of zeros inside it**. As the right pointer expands the window by one element at a time, the number of violations can increase by at most one. This allows us to efficiently maintain a valid window using two pointers. The standard solution shrinks the window with a `while` loop, while this particular problem admits an optimized `if` version because the window's violation can never exceed the limit by more than one in a single step. This makes it a classic example of a **variable-size sliding window**.
