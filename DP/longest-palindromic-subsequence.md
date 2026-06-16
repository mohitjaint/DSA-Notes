# Longest Palindromic Subsequence (LPS)

## References

Question:

[https://leetcode.com/problems/longest-palindromic-subsequence/](https://leetcode.com/problems/longest-palindromic-subsequence/)

Related Problems:

[https://leetcode.com/problems/longest-common-subsequence/](https://leetcode.com/problems/longest-common-subsequence/)

[https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/](https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/)

[https://leetcode.com/problems/delete-operation-for-two-strings/](https://leetcode.com/problems/delete-operation-for-two-strings/)

[https://leetcode.com/problems/uncrossed-lines/](https://leetcode.com/problems/uncrossed-lines/)

---

# Pattern

```txt
LPS = LCS(original_string, reversed_string)

String DP

LCS Pattern
```

---

# Key Observation

A palindrome reads the same forward and backward.

Example:

```txt
bbbab
```

Reverse:

```txt
babbb
```

Any palindromic subsequence appears in both strings.

Therefore:

```txt
Longest Palindromic Subsequence

=

Longest Common Subsequence

between

s
and
reverse(s)
```

---

# Transformation

```cpp
string s1 = s;

reverse(s.begin(), s.end());

string s2 = s;
```

Now find:

```txt
LCS(s1, s2)
```

---

# State

```cpp
dp[i][j]
```

Meaning:

```txt
Length of LCS

between

s1[0...i-1]
and

s2[0...j-1]
```

---

# Base Case

```cpp
dp[0][*] = 0
dp[*][0] = 0
```

Empty string contributes nothing.

---

# Transition

## Match

```cpp
if(s1[i-1] == s2[j-1])
```

```cpp
dp[i][j]
=
dp[i-1][j-1] + 1
```

---

## Mismatch

```cpp
dp[i][j]
=
max(
    dp[i-1][j],
    dp[i][j-1]
)
```

---

# Answer

```cpp
dp[n][n]
```

---

# Complexity (2D DP)

Time:

```txt
O(n²)
```

Space:

```txt
O(n²)
```

---

# Space Optimization

Observation:

```cpp
dp[i][j]
```

depends on:

```cpp
dp[i-1][j]
dp[i][j-1]
dp[i-1][j-1]
```

Therefore:

```txt
1D DP Possible
```

but we must preserve:

```cpp
dp[i-1][j-1]
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

After update:

```txt
dp[i][j]
```

---

# The Prev Diagonal Trick

We need:

```cpp
dp[i-1][j-1]
```

which gets overwritten.

Store it in:

```cpp
prevDiag
```

---

# Correct 1D DP

```cpp
for(int i = 1; i <= n; i++){

    int prevDiag = 0;

    for(int j = 1; j <= n; j++){

        int temp = dp[j];

        if(s1[i-1] == s2[j-1]){
            dp[j] = prevDiag + 1;
        }
        else{
            dp[j] = max(dp[j], dp[j-1]);
        }

        prevDiag = temp;
    }
}
```

---

# Understanding The Variables

Before update:

```cpp
dp[j]
```

stores:

```txt
dp[i-1][j]
```

---

```cpp
dp[j-1]
```

stores:

```txt
dp[i][j-1]
```

because current row has already been computed.

---

```cpp
prevDiag
```

stores:

```txt
dp[i-1][j-1]
```

which is required in the match case.

---

# Why Save temp?

Before updating:

```cpp
temp = dp[j];
```

stores:

```txt
dp[i-1][j]
```

After computation:

```cpp
prevDiag = temp;
```

so next column can use it as:

```txt
dp[i-1][j-1]
```

---

# Common Mistake #1

Using backward iteration:

```cpp
for(j = n; j >= 1; j--)
```

Wrong for LCS.

Reason:

```txt
Need current row's dp[j-1]

i.e.

dp[i][j-1]
```

which is unavailable when moving backward.

---

# Common Mistake #2

Using:

```cpp
dp[j] = dp[j-1] + 1
```

for match.

Wrong.

Because:

```cpp
dp[j-1]
```

contains:

```txt
dp[i][j-1]
```

not:

```txt
dp[i-1][j-1]
```

Need:

```cpp
prevDiag + 1
```

---

# Common Mistake #3

Confusing LCS with Longest Common Substring

LCS:

```cpp
if(match)
    prevDiag + 1
else
    max(dp[j], dp[j-1])
```

---

Longest Common Substring:

```cpp
if(match)
    dp[j-1] + 1
else
    0
```

Different problem.

---

# Complexity

## 2D DP

Time:

```txt
O(n²)
```

Space:

```txt
O(n²)
```

---

## 1D DP

Time:

```txt
O(n²)
```

Space:

```txt
O(n)
```

---

# Pattern Recognition

Whenever you see:

```txt
Palindrome

Subsequence

Longest Palindromic Subsequence
```

Think:

```txt
Reverse String

↓

LCS
```

---

# Related Problems

Same LCS Pattern:

[https://leetcode.com/problems/longest-common-subsequence/](https://leetcode.com/problems/longest-common-subsequence/)

[https://leetcode.com/problems/delete-operation-for-two-strings/](https://leetcode.com/problems/delete-operation-for-two-strings/)

[https://leetcode.com/problems/uncrossed-lines/](https://leetcode.com/problems/uncrossed-lines/)

[https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/](https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/)

---

# Interview Trigger

```txt
Longest Palindromic Subsequence

↓

Reverse String

↓

LCS

↓

1D DP Possible

Use:

prevDiag

to preserve

dp[i-1][j-1]
```

---

## DP Compression Patterns Learned So Far

| Problem                  | Iteration | Extra Variable |
| ------------------------ | --------- | -------------- |
| 0/1 Knapsack             | Backward  | No             |
| Unbounded Knapsack       | Forward   | No             |
| Coin Change II           | Forward   | No             |
| Longest Common Substring | Backward  | No             |
| LCS / LPS                | Forward   | `prevDiag`     |

This `prevDiag` trick is one of the most important DP compression techniques in string DP.
