# Longest Repeating Character Replacement (Sliding Window Notes)

## References

**Question:**

[https://leetcode.com/problems/longest-repeating-character-replacement/](https://leetcode.com/problems/longest-repeating-character-replacement/)

**Related Problems:**

- Longest Substring Without Repeating Characters
- Max Consecutive Ones III
- Fruit Into Baskets
- Minimum Window Substring

---

# Pattern

```text id="p1"
Sliding Window
Variable Size Window
Frequency Tracking
Max Frequency Optimization
```

---

# Problem Statement

You are allowed to replace at most `k` characters in a string.

Find the length of the longest substring that can be converted into a string with all identical characters.

---

# Key Insight

To make a substring valid:

```text id="p2"
changes needed = window size - max frequency character
```

So condition becomes:

```text id="p3"
(window size - maxFreq) ≤ k
```

---

# Sliding Window State

Maintain:

```cpp id="p4"
l → left pointer
r → right pointer
freq → frequency of characters in window
maxFreq → most frequent character count in window
```

---

# Core Idea

We try to **maximize window size**, while ensuring:

```text id="p5"
we can convert all characters into one using ≤ k replacements
```

---

# Expansion Step

For every character:

```cpp id="p6"
freq[s[r]]++
maxFreq = max(maxFreq, freq[s[r]])
```

---

# Validity Condition

A window is invalid if:

```text id="p7"
(r - l + 1) - maxFreq > k
```

---

# Shrinking Step (Two Valid Approaches)

## 1. Standard (safer)

```cpp id="p8"
while ((r - l + 1) - maxFreq > k) {
    freq[s[l]]--;
    l++;
}
```

✔ Always restores validity
✔ General template

---

## 2. Optimized (your version)

```cpp id="p9"
if ((r - l + 1) - maxFreq > k) {
    freq[s[l]]--;
    l++;
}
```

✔ Works here because violation increases slowly
✔ Only one shrink needed per iteration

---

# Updating Answer

Two equivalent styles:

## Safe style

```cpp id="p10"
after fixing window:
ans = max(ans, r - l + 1);
```

## Your style

```cpp id="p11"
if (window is valid)
    ans = max(ans, r - l + 1);
```

---

# Why `maxFreq` is NOT decreased when shrinking

Important trick:

```text id="p12"
maxFreq is allowed to be stale
```

Even if frequency decreases, keeping old maxFreq:

- does not break correctness
- avoids recomputation
- maintains O(n)

---

# Complexity

```text id="p13"
Time  : O(n)
Space : O(1) (26 letters / ASCII)
```

---

# Why Sliding Window Works Here

Because:

```text id="p14"
Adding one character changes window validity by at most 1
```

So adjustments are local and incremental.

---

# Common Mistakes

## ❌ Mistake 1: recomputing maxFreq every time

Too slow → O(26n) or worse.

---

## ❌ Mistake 2: shrinking incorrectly

Using `while` in optimized version unnecessarily.

---

## ❌ Mistake 3: thinking maxFreq must be exact

It only needs to be an upper bound.

---

# Pattern Recognition

Whenever you see:

```text id="p15"
Make all characters same using ≤ k changes
```

Think:

```text id="p16"
Sliding Window + Frequency + maxFreq trick
```

---

# Interview Trigger

```text id="p17"
Substring + Replace k characters

↓

Track frequency

↓

Maintain maxFreq

↓

Check (window size - maxFreq)

↓

Slide window

↓

Return max length
```

---

# Core Insight

Instead of explicitly trying all replacements, we track how dominant a character is in the current window. If all other characters can be converted into this dominant one using at most `k` replacements, the window is valid. This reduces a potentially complex combinational problem into a simple linear sliding window with frequency tracking and a clever use of `maxFreq`.
