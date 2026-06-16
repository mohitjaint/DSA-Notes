# Longest Common Substring

## References

Question:

[https://www.geeksforgeeks.org/problems/longest-common-substring1452/1](https://www.geeksforgeeks.org/problems/longest-common-substring1452/1)

Related Problems:

[https://leetcode.com/problems/longest-common-subsequence/](https://leetcode.com/problems/longest-common-subsequence/)

[https://leetcode.com/problems/maximum-length-of-repeated-subarray/](https://leetcode.com/problems/maximum-length-of-repeated-subarray/)

[https://www.geeksforgeeks.org/problems/print-longest-common-subsequence/1](https://www.geeksforgeeks.org/problems/print-longest-common-subsequence/1)

---

# Pattern

```txt
DP on Strings

Longest Common Substring

Contiguous Matching Required
```

---

# Key Observation

A **substring** must be:

```txt
Continuous
Contiguous
```

Unlike LCS:

```txt
Characters cannot be skipped
inside the matching segment.
```

---

## Example

```txt
s1 = "abcde"
s2 = "ace"
```

### Longest Common Subsequence

```txt
ace
```

Length:

```txt
3
```

---

### Longest Common Substring

Possible common substrings:

```txt
a
c
e
```

Answer:

```txt
1
```

because:

```txt
Substring must be contiguous.
```

---

# State

```cpp
dp[i][j]
```

Meaning:

```txt
Length of longest common substring

ending at

s1[i-1]
and
s2[j-1]
```

The phrase:

```txt
ENDING AT
```

is extremely important.

---

# Base Case

```cpp
dp[0][*] = 0
dp[*][0] = 0
```

Empty string cannot contribute.

---

# Transition

## Match

If:

```cpp
s1[i-1] == s2[j-1]
```

Then current substring can be extended:

```cpp
dp[i][j]
=
dp[i-1][j-1] + 1
```

---

## Mismatch

If:

```cpp
s1[i-1] != s2[j-1]
```

Current substring breaks.

Therefore:

```cpp
dp[i][j] = 0
```

---

# Why Reset To Zero?

Example:

```txt
s1 = "abc"
s2 = "adc"
```

At:

```txt
b != d
```

Any common substring ending here is impossible.

Therefore:

```cpp
dp[i][j] = 0
```

Unlike LCS:

```txt
Cannot carry previous answers.
```

---

# Final Answer

The answer is:

```cpp
max(dp[i][j])
```

over all cells.

Not:

```cpp
dp[n][m]
```

because the longest common substring may end anywhere.

---

## Example

```txt
s1 = "xyzabc"
s2 = "pqrabc"
```

DP peak:

```txt
abc
```

Length:

```txt
3
```

Answer:

```txt
max(dp)
```

---

# 2D DP Solution

```cpp
for(int i=1;i<=n;i++){
    for(int j=1;j<=m;j++){

        if(s1[i-1] == s2[j-1])
            dp[i][j] = dp[i-1][j-1] + 1;
        else
            dp[i][j] = 0;

        ans = max(ans, dp[i][j]);
    }
}
```

---

# Space Optimization

Observation:

```cpp
dp[i][j]
=
dp[i-1][j-1] + 1
```

Only the previous diagonal is needed.

Therefore:

```txt
1D DP Possible
```

---

# 1D State

```cpp
dp[j]
```

Meaning:

```txt
Current row values
```

---

# Important Iteration Order

```cpp
for(j = m; j >= 1; j--)
```

Backward iteration.

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

must still contain:

```txt
Previous row value
```

and must not be overwritten.

---

## Correct 1D Transition

```cpp
if(s1[i-1] == s2[j-1])
    dp[j] = dp[j-1] + 1;
else
    dp[j] = 0;
```

---

# Common Mistake #1

Forgetting:

```cpp
dp[j] = 0;
```

on mismatch.

Wrong:

```cpp
if(match)
    dp[j] = dp[j-1] + 1;
```

without:

```cpp
else
    dp[j] = 0;
```

---

### Why Wrong?

Old values remain:

```txt
Substring continues
through mismatch
```

which is impossible.

---

# Common Mistake #2

Using Forward Iteration

Wrong:

```cpp
for(j=1;j<=m;j++)
```

Then:

```cpp
dp[j-1]
```

has already been updated.

You lose:

```cpp
dp[i-1][j-1]
```

---

# Common Mistake #3

Returning:

```cpp
dp[n][m]
```

Wrong.

Longest common substring may end anywhere.

Need:

```cpp
max(dp[i][j])
```

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

# Comparison With LCS

| Property                | LCS          | Longest Common Substring |
| ----------------------- | ------------ | ------------------------ |
| Skip Characters Allowed | ✅           | ❌                       |
| Contiguous Required     | ❌           | ✅                       |
| Mismatch Transition     | max(...)     | 0                        |
| Answer                  | dp[n][m]     | max(dp)                  |
| 1D Iteration            | More complex | Backward                 |

---

# Pattern Recognition

Whenever you see:

```txt
Longest Common

Substring

Continuous

Contiguous
```

Think:

```txt
dp[i][j]

=
Length of matching suffix

ending at i,j
```

and immediately write:

```cpp
if(match)
    dp[i-1][j-1] + 1;
else
    0;
```

---

# Interview Trigger

```txt
If you see:

Substring

Contiguous matching

Two strings

Longest common segment

Think:

Longest Common Substring

State:
dp[i][j]

=
Length of common suffix
ending at i,j

Mismatch
→ Reset to 0

Answer
→ max(dp)
```

---

### Connection to Other DP Problems

```txt
LCS
→ Can skip characters

Longest Common Substring
→ Cannot skip characters

Maximum Length Repeated Subarray
→ Same DP pattern

Longest Common Prefix
→ Greedy/String pattern (not DP)
```

This is one of the easiest string DPs once you remember the phrase:

```txt
"Substring = Contiguous = Reset to 0 on mismatch"
```
