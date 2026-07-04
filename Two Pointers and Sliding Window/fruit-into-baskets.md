# 🍎 Fruit Into Baskets (LeetCode 904) — Sliding Window Notes

## References

**Question:**

[https://leetcode.com/problems/fruit-into-baskets/](https://leetcode.com/problems/fruit-into-baskets/)

**Related Problems:**

- Longest Substring Without Repeating Characters
- Longest Repeating Character Replacement
- Max Consecutive Ones III
- Subarrays with K Distinct Elements

---

# 🧠 Pattern

```text id="p1"
Sliding Window
Variable Size Window
At Most K Distinct Elements (K = 2)
Frequency Map
```

---

# 📌 Problem Statement

You are given an array `fruits`.

Each index represents a tree type.

You can pick fruits using **at most 2 baskets**, and each basket can hold only one type.

Goal:

```text id="p2"
Find the longest contiguous subarray with at most 2 distinct numbers.
```

---

# 🔑 Key Insight

We want the **largest window with ≤ 2 distinct elements**.

So condition is:

```text id="p3"
number of distinct fruits ≤ 2
```

---

# 🧠 Sliding Window State

Maintain:

```cpp id="p4"
l → left pointer
r → right pointer
freq → frequency map of current window
```

---

# 🚀 Core Idea

We expand `r`, and ensure window always stays valid:

```text id="p5"
valid window = at most 2 distinct fruits
```

---

# 📈 Expansion Step

```cpp id="p6"
freq[fruits[r]]++
```

---

# ❌ Invalid Condition

If:

```text id="p7"
freq.size() > 2
```

window becomes invalid.

---

# 🔧 Shrinking Step

```cpp id="p8"
while (freq.size() > 2) {
    freq[fruits[l]]--;
    if (freq[fruits[l]] == 0)
        freq.erase(fruits[l]);
    l++;
}
```

---

# 📊 Update Answer

After window is valid:

```cpp id="p9"
ans = max(ans, r - l + 1);
```

---

# ⚡ Final Template

```cpp id="p10"
for (int r = 0; r < n; r++) {

    freq[fruits[r]]++;

    while (freq.size() > 2) {
        freq[fruits[l]]--;
        if (freq[fruits[l]] == 0)
            freq.erase(fruits[l]);
        l++;
    }

    ans = max(ans, r - l + 1);
}
```

---

# 🧠 Why `while` is necessary here

Unlike previous problems:

- violation can grow beyond 1
- multiple fruit types may exist at once
- removing one element may not fix constraint

So:

```text id="p11"
need full cleanup → while loop
```

---

# ⚖️ Comparison with Similar Problems

| Problem                  | Constraint        | Technique        |
| ------------------------ | ----------------- | ---------------- |
| Max Consecutive Ones III | ≤ k zeros         | window + counter |
| Character Replacement    | replace ≤ k chars | maxFreq trick    |
| Fruit Into Baskets       | ≤ 2 distinct      | freq map + while |
| Longest Substring        | no duplicates     | last index map   |

---

# ❗ Common Mistakes

### ❌ Mistake 1: using `if` instead of `while`

Wrong because multiple removals may be needed.

---

### ❌ Mistake 2: tracking last index instead of frequency

This problem depends on **count**, not position.

---

### ❌ Mistake 3: forgetting to erase when freq becomes 0

```cpp id="p12"
if(freq[x] == 0) erase(x);
```

---

# 🧠 Why this is different from “Character Replacement”

| Fruit Basket         | Character Replacement |
| -------------------- | --------------------- |
| need ≤ 2 types       | need ≤ k replacements |
| freq size constraint | maxFreq constraint    |
| strict invalidity    | soft constraint       |

---

# 🚀 Interview Trigger

```text id="p13"
Subarray + At most K distinct

↓

Use frequency map

↓

Expand r

↓

Shrink l while invalid

↓

Track max length
```

---

# 🧠 Core Insight

We are not tracking fruit positions or patterns. Instead, we only care about how many distinct fruit types are present in the current window. As long as this number is ≤ 2, we can keep extending the window. Once it exceeds 2, we greedily shrink from the left until the constraint is restored. This ensures we always maintain the longest valid contiguous segment in linear time.
