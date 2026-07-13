# Longest K Unique Characters Substring (GeeksforGeeks)

## References

**Question:**

[https://www.geeksforgeeks.org/problems/longest-k-unique-characters-substring0853/1](https://www.geeksforgeeks.org/problems/longest-k-unique-characters-substring0853/1)

**Related Problems:**

- Fruit Into Baskets
- Longest Substring Without Repeating Characters
- Longest Repeating Character Replacement
- Subarrays with K Distinct Integers

---

# Pattern

```text
Sliding Window

Variable Size Window

Exactly K Distinct Characters
```

---

# Problem Statement

Given a string,

find the length of the **longest substring containing exactly K distinct characters**.

If no such substring exists,

return

```text
-1
```

---

# Brute Force

Generate every substring.

Maintain a frequency map.

Count distinct characters.

Update answer only when

```text
distinct == k
```

---

## Complexity

```text
Time  : O(n²)

Space : O(Σ)
```

where Σ is the character set.

---

# Key Observation

A substring is valid only when

```text
distinct characters ≤ k
```

If distinct characters become greater than

```text
k
```

the window must be shrunk.

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
freq
```

Frequency map of characters in current window.

---

# Window Invariant

```text
Window

[l........r]

contains at most K distinct characters.
```

---

# Expansion

Include

```cpp
s[r]
```

```cpp
freq[s[r]]++;
```

---

# Shrinking

Whenever

```text
freq.size() > k
```

```cpp
while(freq.size() > k){

    freq[s[l]]--;

    if(freq[s[l]] == 0)
        freq.erase(s[l]);

    l++;
}
```

After shrinking,

the window again satisfies

```text
distinct ≤ k
```

---

# Updating Answer

Unlike "at most K" problems,

we only update when

```text
distinct == k
```

```cpp
if(freq.size() == k)
    ans = max(ans, r-l+1);
```

---

# Complete Algorithm

```text
Expand right pointer

↓

If distinct > K

↓

Shrink left pointer

↓

If distinct == K

↓

Update answer

↓

Repeat
```

---

# Complexity

```text
Time : O(n)

Space : O(Σ)
```

Each character enters and leaves the window at most once.

---

# Why It Works

The window always satisfies

```text
distinct ≤ k
```

Whenever

```text
distinct == k
```

the current window is a valid candidate.

Since every possible valid window is visited once,

the maximum length is obtained.

---

# Difference from Fruit Into Baskets

Fruit Into Baskets asks for

```text
At most 2 distinct
```

so every valid window contributes.

```cpp
ans = max(ans, r-l+1);
```

Longest K Unique asks for

```text
Exactly K distinct
```

so update only when

```cpp
freq.size() == k
```

---

# Comparison

| Problem                             | Window Invariant              | Update Condition             |
| ----------------------------------- | ----------------------------- | ---------------------------- |
| Fruit Into Baskets                  | distinct ≤ 2                  | Always update                |
| Longest K Unique Characters         | distinct ≤ K                  | Update only if distinct == K |
| Longest Substring Without Repeating | no duplicates                 | Always update                |
| Character Replacement               | `(window size - maxFreq) ≤ K` | Always update                |

---

# Common Mistake #1

Updating answer whenever

```text
distinct ≤ k
```

Wrong.

The question asks for **exactly** `k` distinct characters.

---

# Common Mistake #2

Returning

```text
0
```

when no valid substring exists.

The required answer is

```text
-1
```

Initialize

```cpp
int ans = -1;
```

---

# Common Mistake #3

Forgetting to erase characters when frequency becomes zero.

```cpp
if(freq[ch] == 0)
    freq.erase(ch);
```

Otherwise,

```cpp
freq.size()
```

becomes incorrect.

---

# Pattern Recognition

Whenever you see

```text
Longest Substring

+

Exactly K Distinct
```

think

```text
Sliding Window

↓

Maintain

distinct ≤ K

↓

Shrink when

distinct > K

↓

Update only when

distinct == K
```

---

# Interview Trigger

```text
Longest Substring

↓

Exactly K Distinct

↓

Frequency Map

↓

Expand right

↓

Shrink while distinct > K

↓

Update only if distinct == K

↓

O(n)
```

---

# Core Insight

This problem is a direct extension of the **"At Most K Distinct"** sliding window template. Instead of trying to maintain **exactly** `K` distinct characters throughout the window, maintain the easier invariant **at most `K` distinct characters**. Whenever the window contains exactly `K` distinct characters, it becomes a valid candidate for the answer. This separation of **window maintenance** (`≤ K`) and **answer condition** (`== K`) is the key idea behind many sliding window problems.
