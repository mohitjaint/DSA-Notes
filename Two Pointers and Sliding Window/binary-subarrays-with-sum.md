# 🔥 Number of Subarrays With Sum (LeetCode 930) — Notes

## References

**Question:**
[https://leetcode.com/problems/binary-subarrays-with-sum/](https://leetcode.com/problems/binary-subarrays-with-sum/)

**Related Problems:**

- Subarray Sum Equals K
- Count Number of Nice Subarrays
- Longest Subarray with Sum K
- Sliding Window Maximum (pattern contrast)

---

# 🧠 Pattern

```text
Binary Array + Sum Constraint
→ Prefix Sliding Window Trick
→ atMost(K) - atMost(K-1)
```

---

# 📌 Problem Statement

Given a binary array, count the number of subarrays whose sum equals `goal`.

---

# ❌ Why Direct Sliding Window Fails

If you try:

```text
sum == goal
```

You miss multiple valid subarrays ending at the same `r`.

Because:

> For one right endpoint, multiple left endpoints may form valid subarrays.

---

# 🧠 Key Insight

Instead of counting exact sum directly:

```text
exact(goal)
= atMost(goal) - atMost(goal - 1)
```

---

# 🚀 Core Idea

Convert the problem into:

### Step 1

Count subarrays with sum ≤ goal

### Step 2

Count subarrays with sum ≤ goal-1

### Step 3

Subtract

---

# 📌 Function: atMost(K)

```cpp
int f(nums, goal)
```

Meaning:

> Number of subarrays with sum ≤ goal

---

# 🧠 Sliding Window Logic

## State

```cpp
l = left pointer
r = right pointer
sum = current window sum
```

---

## Expand window

```cpp
sum += nums[r]
```

---

## Shrink window

```cpp
while (sum > goal) {
    sum -= nums[l];
    l++;
}
```

---

## Count valid subarrays

```cpp
cnt += (r - l + 1);
```

Because all subarrays ending at `r` starting from `[l ... r]` are valid.

---

# 📌 Final Answer

```cpp
return atMost(goal) - atMost(goal - 1);
```

---

# ⚡ Why Counting Works

For fixed `r`:

```text
All valid starts = l, l+1, ..., r
Count = r - l + 1
```

---

# 📊 Complexity

| Metric | Value |
| ------ | ----- |
| Time   | O(n)  |
| Space  | O(1)  |

Each element enters and exits the window once.

---

# 🧠 Key Pattern Recognition

If you see:

```text
Binary array + “sum = K”
```

DO NOT think:

❌ direct sliding window for exact sum

Instead think:

```text
✔ atMost(K) - atMost(K-1)
```

---

# ⚠️ Edge Case

```cpp
if(goal < 0) return 0;
```

Reason:

```text
No subarray can have negative sum in binary array
```

---

# 🔥 Common Mistakes

## ❌ Mistake 1

Trying to maintain exact sum window

## ❌ Mistake 2

Counting only when sum == goal

## ❌ Mistake 3

Forgetting multiple valid left boundaries

---

# 🧠 Core Insight

Instead of tracking _exact matches_, we transform the problem into counting _bounded ranges_. This converts a complex “exact sum” condition into a simple linear sliding window problem where each right endpoint contributes a predictable number of valid subarrays.
