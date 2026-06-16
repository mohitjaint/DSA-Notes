# Print All LCS Sequences

## References

Question:

[https://www.geeksforgeeks.org/problems/print-all-lcs-sequences3413/1](https://www.geeksforgeeks.org/problems/print-all-lcs-sequences3413/1)

Related Problems:

[https://leetcode.com/problems/longest-common-subsequence/](https://leetcode.com/problems/longest-common-subsequence/)

[https://www.geeksforgeeks.org/problems/print-longest-common-subsequence/1](https://www.geeksforgeeks.org/problems/print-longest-common-subsequence/1)

[https://www.geeksforgeeks.org/problems/shortest-common-supersequence0322/1](https://www.geeksforgeeks.org/problems/shortest-common-supersequence0322/1)

---

# Pattern

```txt
LCS Reconstruction

Memoized DFS

State Returns Collection of Answers
```

---

# Problem Statement

Given two strings:

```txt
s1
s2
```

Return:

```txt
All distinct Longest Common Subsequences
```

in:

```txt
Lexicographical Order
```

---

# Why Normal LCS DP Is Not Enough?

Normal LCS gives:

```cpp
dp[i][j]
```

Meaning:

```txt
Length of LCS
```

Example:

```txt
ABCBDAB
BDCABA
```

LCS Length:

```txt
4
```

But there are multiple valid LCS strings:

```txt
BCAB
BDAB
BCBA
...
```

Need:

```txt
All of them
```

---

# First Thought (Wrong)

Use:

```cpp
dfs(i,j,currentString)
```

and backtrack.

Problem:

```txt
Same state
(i,j)
```

gets visited repeatedly.

---

# Memoization Insight

Instead of:

```cpp
dfs(i,j,currentString)
```

define:

```cpp
dfs(i,j)
```

Meaning:

```txt
Return all LCS strings possible from

s1[0...i-1]
s2[0...j-1]
```

Return Type:

```cpp
unordered_set<string>
```

---

# State Definition

```cpp
memo[i][j]
```

stores:

```txt
All distinct LCS strings
obtainable from

s1[0...i-1]
s2[0...j-1]
```

---

# Base Case

If:

```cpp
i == 0 || j == 0
```

Return:

```cpp
{""}
```

Meaning:

```txt
One empty subsequence
```

---

# Match Case

If:

```cpp
s1[i-1] == s2[j-1]
```

Then:

```cpp
prev = dfs(i-1,j-1)
```

Append current character to every string:

```cpp
for(str : prev)
    ans.insert(str + s1[i-1]);
```

---

## Why?

LCS recurrence:

```cpp
dp[i][j]
=
dp[i-1][j-1] + 1
```

Current character must belong to the LCS.

---

# Non-Match Case

If:

```cpp
s1[i-1] != s2[j-1]
```

We follow only those states that preserve LCS length.

---

## Go Up

```cpp
if(dp[i-1][j] == dp[i][j])
```

Merge:

```cpp
dfs(i-1,j)
```

---

## Go Left

```cpp
if(dp[i][j-1] == dp[i][j])
```

Merge:

```cpp
dfs(i,j-1)
```

---

## Important

Use:

```cpp
if
if
```

NOT:

```cpp
if
else if
```

because both branches may contribute valid LCS strings.

---

# Memoization

Before solving:

```cpp
dfs(i,j)
```

check:

```cpp
if(vis[i][j])
    return memo[i][j];
```

After computation:

```cpp
memo[i][j] = ans;
vis[i][j] = true;
```

---

# Why Memoization Works?

State:

```txt
(i,j)
```

uniquely determines:

```txt
All LCS strings obtainable
from these prefixes
```

Therefore:

```txt
Valid DP State ✓
```

---

# Common Mistake #1

Trying to memoize:

```cpp
dfs(i,j,currentString)
```

using:

```cpp
(i,j)
```

only.

Wrong.

State actually becomes:

```txt
(i,j,currentString)
```

which is not memoizable efficiently.

---

# Common Mistake #2

Using:

```cpp
if
else if
```

for branching.

Wrong.

Example:

```txt
dp[i-1][j] == dp[i][j]

and

dp[i][j-1] == dp[i][j]
```

Both branches may contain valid LCS strings.

Need:

```cpp
if(...)
if(...)
```

---

# Common Mistake #3

Using Vector Instead of Set

Different DP paths may generate:

```txt
abc
```

multiple times.

Need:

```cpp
unordered_set<string>
```

or:

```cpp
set<string>
```

to remove duplicates.

---

# Complexity

Let:

```txt
n = |s1|
m = |s2|

L = length of LCS

K = number of distinct LCS strings
```

---

## DP Length Computation

Time:

```txt
O(nm)
```

Space:

```txt
O(nm)
```

---

## Reconstruction

Each state stores:

```txt
Collection of strings
```

Complexity:

```txt
O(nm × K × L)
```

Space:

```txt
O(nm × K × L)
```

---

# Important Observation

This problem is:

```txt
Output Sensitive
```

because:

```txt
All LCS strings
must be generated.
```

Even printing them requires:

```txt
Ω(K × L)
```

time.

---

# Editorial Approach

Editorial uses:

```cpp
dp[i][j]
=
Length of LCS
```

only.

No:

```cpp
set<string>
```

stored in DP.

Then reconstructs answers using:

```cpp
Backtracking
+
Character Enumeration
```

---

## Comparison

### Memoized Set DP (Your Solution)

```txt
Easy to derive

State:
(i,j) -> set<string>

Higher memory
```

---

### Editorial Solution

```txt
Store only lengths

Lower memory

Harder to derive
```

---

# Pattern Recognition

Whenever you see:

```txt
Print All Optimal Answers

Print All LCS

Print All Paths

Word Break II

Expression Generation
```

Think:

```txt
Memoized DFS

State returns

set<string>
vector<string>

instead of int/bool
```

---

# Interview Trigger

```txt
Need ALL optimal solutions

Not just count

Not just one answer

Think:

DP + Reconstruction

State:

(i,j)
→ Collection of Answers

Memoization required
to avoid recomputing states.
```
