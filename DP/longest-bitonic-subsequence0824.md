# Longest Bitonic Subsequence (LBS)

## References

**Question:**

[https://www.geeksforgeeks.org/problems/longest-bitonic-subsequence0824/1](https://www.geeksforgeeks.org/problems/longest-bitonic-subsequence0824/1)

**Related Problems:**

- Longest Increasing Subsequence
- Largest Divisible Subset
- Longest String Chain
- Mountain Array

---

# Pattern

```text
LIS Variation

Forward DP

Backward DP

Combine Two DP Arrays
```

---

# Problem Statement

Find the length of the **Longest Bitonic Subsequence**.

A Bitonic Subsequence

- first strictly increases,
- then strictly decreases.

Both increasing and decreasing parts must exist.

---

# Key Observation

A bitonic sequence has a **peak**.

```text
        Peak
          ▲
         / \
        /   \
       /     \
```

If we know

- longest increasing subsequence ending at the peak,
- longest decreasing subsequence starting from the peak,

then we can compute the answer for that peak.

---

# DP State

## Forward DP

```cpp
inc[i]
```

Meaning

```text
Length of LIS

ending at index i.
```

---

## Backward DP

```cpp
dec[i]
```

Meaning

```text
Length of LDS

starting from index i.
```

---

# Computing LIS

Exactly the standard LIS DP.

Initialization

```cpp
inc[i] = 1;
```

Transition

```cpp
for(int i=0;i<n;i++){

    for(int j=0;j<i;j++){

        if(nums[j]<nums[i]){

            inc[i]=max(
                inc[i],
                inc[j]+1
            );
        }
    }
}
```

---

# Computing LDS

Run LIS in reverse.

Initialization

```cpp
dec[i]=1;
```

Transition

```cpp
for(int i=n-1;i>=0;i--){

    for(int j=n-1;j>i;j--){

        if(nums[j]<nums[i]){

            dec[i]=max(
                dec[i],
                dec[j]+1
            );
        }
    }
}
```

---

# Combining Both

Treat every index as the peak.

```text
Increasing

↓

Peak

↓

Decreasing
```

Length

```cpp
inc[i] + dec[i] - 1
```

Subtract one because

```text
Peak

is counted twice.
```

Answer

```cpp
ans=max(ans,inc[i]+dec[i]-1);
```

---

# Your Implementation

You stored

```cpp
dpf[i]
```

Meaning

```text
LIS length - 1
```

and

```cpp
dpb[i]
```

Meaning

```text
LDS length - 1
```

Therefore

```cpp
dpf[i] + dpb[i] + 1
```

is equivalent to

```text
inc[i] + dec[i] - 1
```

Both approaches are mathematically identical.

---

# Why Ignore Some Peaks?

A valid bitonic sequence must

- increase at least once,
- decrease at least once.

Therefore

```cpp
if(dpf[i]==0 || dpb[i]==0)
    continue;
```

or equivalently

```cpp
if(inc[i]==1 || dec[i]==1)
    continue;
```

This excludes

```text
Only Increasing

Only Decreasing

Single Element
```

since they are not bitonic.

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

LIS computes

```text
Best sequence

ending at i
```

Bitonic computes

```text
Best Increasing

ending at i

+

Best Decreasing

starting at i
```

The peak joins the two sequences.

---

# Common Mistake #1

Forgetting to compute the backward DP.

A single LIS is not sufficient.

---

# Common Mistake #2

Returning

```cpp
inc[i] + dec[i]
```

The peak gets counted twice.

Correct

```cpp
inc[i] + dec[i] - 1
```

---

# Common Mistake #3

Allowing

```text
Purely Increasing

or

Purely Decreasing
```

to be considered bitonic.

Both parts must exist.

---

# Common Mistake #4

Running the second DP in the wrong direction.

The decreasing DP depends on future indices.

Correct order

```text
n-1

↓

0
```

---

# Pattern Recognition

Whenever a problem asks

```text
Increase

↓

Peak

↓

Decrease
```

think

```text
Forward DP

+

Backward DP

+

Combine at every index
```

---

# Interview Trigger

```text
Longest Bitonic Subsequence

↓

Peak Observation

↓

LIS ending at every index

↓

LDS starting at every index

↓

Every index is a candidate peak

↓

Combine

inc[i] + dec[i] - 1

↓

O(n²)
```

---

# Core Insight

A bitonic subsequence can be decomposed into **two independent DP problems**:

- the **Longest Increasing Subsequence** ending at each index,
- the **Longest Decreasing Subsequence** starting from each index.

Every element is treated as the potential peak, and the two lengths are combined while counting the peak only once. This is a classic example of solving a complex DP problem by **combining the results of two simpler DP computations**.
