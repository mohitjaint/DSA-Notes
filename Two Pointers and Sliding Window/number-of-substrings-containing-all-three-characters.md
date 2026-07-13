# Number of Substrings Containing All Three Characters (LeetCode 1358)

## References

**Question:**

[https://leetcode.com/problems/number-of-substrings-containing-all-three-characters/](https://leetcode.com/problems/number-of-substrings-containing-all-three-characters/)

**Related Problems:**

- Fruit Into Baskets
- Binary Subarrays With Sum
- Count Number of Nice Subarrays
- Longest Repeating Character Replacement

---

# Pattern

```text
Sliding Window

Complement Counting

At Most K Distinct
```

---

# Problem Statement

Given a string consisting of only

```text
'a', 'b', 'c'
```

count the number of substrings containing **all three characters**.

---

# Brute Force

Generate every substring.

Check whether it contains

```text
a, b and c
```

---

## Complexity

```text
Time  : O(n²)

Space : O(1)
```

---

# Key Observation

Every substring belongs to exactly one of the following categories:

```text
1 distinct character

2 distinct characters

3 distinct characters
```

We need category **3**.

Instead of counting it directly,

count everything else.

---

# Complement Formula

```text
Answer

=

Total Substrings

-

Substrings having at most 2 distinct characters
```

---

# Total Number of Substrings

For a string of length

```text
n
```

```text
Total

=

n(n+1)/2
```

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
freq[3]
```

Frequency of

```text
a, b, c
```

```cpp
distinct
```

Number of distinct characters inside the current window.

---

# Window Invariant

```text
Window

[l........r]

contains at most 2 distinct characters.
```

---

# Expansion

```cpp
int idx = s[r] - 'a';

if(freq[idx] == 0)
    distinct++;

freq[idx]++;
```

---

# Shrinking

Whenever

```text
distinct > 2
```

```cpp
while(distinct > 2){

    int idx = s[l] - 'a';

    freq[idx]--;

    if(freq[idx] == 0)
        distinct--;

    l++;
}
```

---

# Counting Valid Windows

After restoring validity,

every subarray ending at

```cpp
r
```

and starting anywhere from

```text
l ... r
```

is valid.

Hence

```cpp
invalid += r-l+1;
```

where

```text
invalid
```

represents

```text
Substrings containing at most 2 distinct characters.
```

---

# Final Answer

```cpp
return totalSubstrings - invalid;
```

---

# Complete Algorithm

```text
Compute total substrings

↓

Count substrings with ≤2 distinct characters

↓

Subtract

↓

Answer
```

---

# Why Frequency Array Instead of Hash Map?

The string contains only

```text
'a'

'b'

'c'
```

So

```cpp
freq[3]
```

is sufficient.

Mapping:

```cpp
'a' → 0

'b' → 1

'c' → 2
```

using

```cpp
int idx = s[i] - 'a';
```

---

# Maintaining Distinct Count

Unlike

```cpp
unordered_map
```

arrays don't know how many different characters are present.

Maintain it manually.

New character enters:

```cpp
if(freq[idx] == 0)
    distinct++;

freq[idx]++;
```

Character disappears:

```cpp
freq[idx]--;

if(freq[idx] == 0)
    distinct--;
```

---

# Complexity

```text
Time : O(n)

Space : O(1)
```

---

# Common Mistake #1

Trying to count substrings containing all three characters directly.

Much harder than counting the complement.

---

# Common Mistake #2

Using

```cpp
unordered_map
```

for only three characters.

A frequency array is simpler and faster.

---

# Common Mistake #3

Forgetting to maintain

```cpp
distinct
```

when using an array.

---

# Pattern Recognition

Whenever the question asks for

```text
Substrings containing ALL required elements
```

check whether

```text
Answer

=

Total

-

Substrings missing something
```

is easier.

Many problems become simple complement-counting problems.

---

# Comparison with Similar Problems

| Problem                                              | Trick                             |
| ---------------------------------------------------- | --------------------------------- |
| Fruit Into Baskets                                   | Longest window with ≤2 distinct   |
| Binary Subarrays With Sum                            | Exactly = AtMost(K) − AtMost(K−1) |
| Count Nice Subarrays                                 | Exactly = AtMost(K) − AtMost(K−1) |
| Number of Substrings Containing All Three Characters | Total − AtMost(2 distinct)        |

---

# Interview Trigger

```text
Substring

↓

Contains all required characters

↓

Complement is easier

↓

Count substrings with ≤ K distinct

↓

Subtract from total

↓

O(n)
```

---

# Core Insight

Instead of directly counting substrings containing **all three characters**, count the easier set: substrings containing **at most two distinct characters**. Since every substring belongs to either the "≤2 distinct" category or the "all 3 distinct" category, subtracting the former from the total number of substrings immediately gives the answer. This is a powerful application of **complement counting with a sliding window**.
