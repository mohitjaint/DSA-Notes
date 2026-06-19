# Distinct Subsequences

## References

Question:

[https://leetcode.com/problems/distinct-subsequences/](https://leetcode.com/problems/distinct-subsequences/)

Related Problems:

[https://leetcode.com/problems/interleaving-string/](https://leetcode.com/problems/interleaving-string/)

[https://leetcode.com/problems/edit-distance/](https://leetcode.com/problems/edit-distance/)

[https://leetcode.com/problems/delete-operation-for-two-strings/](https://leetcode.com/problems/delete-operation-for-two-strings/)

---

# Pattern

```txt
DP on Strings

Count Number of Ways

Subsequence DP
```

---

# Problem Statement

Given:

```txt
Source String : s

Target String : t
```

Find:

```txt
Number of distinct subsequences
of s

that equal t.
```

---

# Key Observation

At every character in `s`, we have two choices:

```txt
Take it

OR

Skip it
```

If the current characters match:

```txt
Both choices are possible.
```

Otherwise:

```txt
Only Skip.
```

---

# Example

```txt
s = rabbbit

t = rabbit
```

Answer:

```txt
3
```

because there are three different ways to remove one `'b'`.

---

# DP State

```cpp
dp[i][j]
```

Meaning:

```txt
Number of ways to form

t[0...j-1]

using

s[0...i-1]
```

---

# Base Cases

## Empty Target

```txt
Target = ""
```

There is exactly one way:

```txt
Take nothing.
```

Therefore:

```cpp
dp[i][0] = 1
```

for every:

```cpp
i
```

---

## Empty Source

```txt
Source = ""
```

Target is non-empty.

Impossible.

Therefore:

```cpp
dp[0][j] = 0
```

for:

```cpp
j > 0
```

---

# Transition

## Characters Match

```cpp
if(s[i-1] == t[j-1])
```

Two choices:

### Take

Use current character.

```cpp
dp[i-1][j-1]
```

---

### Skip

Ignore current character.

```cpp
dp[i-1][j]
```

---

Therefore:

```cpp
dp[i][j]
=
dp[i-1][j-1]
+
dp[i-1][j]
```

---

## Characters Don't Match

Cannot take.

Only skip.

```cpp
dp[i][j]
=
dp[i-1][j]
```

---

# Complete Transition

```cpp
if(s[i-1] == t[j-1])
    dp[i][j]
    =
    dp[i-1][j-1]
    +
    dp[i-1][j];
else
    dp[i][j]
    =
    dp[i-1][j];
```

---

# Why Does It Work?

Suppose:

```txt
Current characters match.
```

Either:

```txt
Use current character

↓

Need remaining target
```

OR

```txt
Ignore current character

↓

Still need same target
```

These two cases are independent.

Hence:

```txt
Addition
```

---

# Space Optimization

Observation:

```cpp
dp[i][j]
```

depends only on:

```cpp
dp[i-1][j]

dp[i-1][j-1]
```

Only previous row.

Therefore:

```txt
1D DP Possible
```

---

# 1D State

```cpp
dp[j]
```

Before update:

```txt
dp[i-1][j]
```

---

# Iteration Order

```cpp
for(j = m; j >= 1; j--)
```

Backward.

---

# Why Backward?

Need:

```cpp
dp[i-1][j-1]
```

When compressed:

```cpp
dp[j-1]
```

must still contain the previous row value.

Forward iteration would overwrite it.

---

# Correct 1D DP

```cpp
dp[0] = 1;

for(int i = 1; i <= n; i++){

    for(int j = m; j >= 1; j--){

        if(s[i-1] == t[j-1]){
            dp[j] += dp[j-1];
        }
    }
}
```

Notice:

```txt
No else required.
```

---

# Why No Else?

Original recurrence:

```cpp
dp[i][j]
=
dp[i-1][j]
```

Before updating,

```cpp
dp[j]
```

already stores:

```txt
dp[i-1][j]
```

So doing nothing is exactly correct.

---

# State During 1D DP

| Value                   | Meaning        |
| ----------------------- | -------------- |
| `dp[j]` (before update) | `dp[i-1][j]`   |
| `dp[j-1]`               | `dp[i-1][j-1]` |
| `dp[j]` (after update)  | `dp[i][j]`     |

---

# Common Mistake #1

Using forward iteration.

Wrong:

```cpp
for(int j=1;j<=m;j++)
```

Reason:

```txt
dp[j-1]

has already become

dp[i][j-1]

instead of

dp[i-1][j-1]
```

---

# Common Mistake #2

Writing an unnecessary else.

Wrong:

```cpp
else
    dp[j] = dp[j];
```

No update is needed.

---

# Common Mistake #3

Using `int`

Intermediate DP values can exceed:

```txt
INT_MAX
```

even if the final answer fits in a 32-bit integer.

This causes:

```txt
Signed Integer Overflow
```

---

# Why Does `double` Work?

LeetCode guarantees:

```txt
Final Answer

fits in

32-bit signed integer.
```

Intermediate DP values may become much larger.

Using:

```cpp
double
```

avoids integer overflow because:

```txt
Huge exponent range

≈ 10^308
```

and integers up to:

```txt
2^53

≈ 9 × 10^15
```

are represented exactly.

This is a practical LeetCode trick, not a general mathematical solution.

---

# Complexity

## 2D DP

Time:

```txt
O(nm)
```

Space:

```txt
O(nm)
```

---

## 1D DP

Time:

```txt
O(nm)
```

Space:

```txt
O(m)
```

---

# Pattern Recognition

Whenever you see:

```txt
Count

Subsequences

Ways

Choose / Skip
```

Think:

```txt
DP

Take

+

Skip
```

Transition:

```cpp
if(match)
    take + skip
else
    skip
```

---

# Related Problems

Same Choose / Skip Pattern:

[https://leetcode.com/problems/interleaving-string/](https://leetcode.com/problems/interleaving-string/)

[https://leetcode.com/problems/edit-distance/](https://leetcode.com/problems/edit-distance/)

[https://leetcode.com/problems/delete-operation-for-two-strings/](https://leetcode.com/problems/delete-operation-for-two-strings/)

---

# Interview Trigger

```txt
If you see:

Count number of subsequences

Two strings

Ways to form target

Think:

State:

dp[i][j]

=

Ways to form

target prefix

using

source prefix

Match

↓

Take + Skip

Mismatch

↓

Skip

Space Optimization

↓

1D DP

Backward Iteration
```

---

# DP Compression Patterns (Updated)

| Problem                  | Transition Depends On                          | Iteration | Extra Variable |
| ------------------------ | ---------------------------------------------- | --------- | -------------- |
| 0/1 Knapsack             | Previous row                                   | Backward  | No             |
| Distinct Subsequences    | Previous row                                   | Backward  | No             |
| Unbounded Knapsack       | Current row                                    | Forward   | No             |
| Coin Change II           | Current row                                    | Forward   | No             |
| Longest Common Substring | Previous diagonal                              | Backward  | No             |
| LCS / LPS                | Previous row + Current row + Previous diagonal | Forward   | `prevDiag`     |

### Pattern Rule

```txt
Need only previous-row states
→ Backward

Need current-row states
→ Forward

Need both previous-row and current-row
→ Forward + prevDiag
```

This rule is much easier to derive during interviews than memorizing iteration orders.
