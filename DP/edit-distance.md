# Edit Distance

## References

Question:

[https://leetcode.com/problems/edit-distance/](https://leetcode.com/problems/edit-distance/)

Related Problems:

[https://leetcode.com/problems/delete-operation-for-two-strings/](https://leetcode.com/problems/delete-operation-for-two-strings/)

[https://leetcode.com/problems/longest-common-subsequence/](https://leetcode.com/problems/longest-common-subsequence/)

[https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/](https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/)

---

# Pattern

```txt
DP on Strings

Transformation DP

Choose Best Operation
```

---

# Problem Statement

Given two strings:

```txt
word1
word2
```

Find the **minimum number of operations** required to convert `word1` into `word2`.

Allowed operations:

```txt
Insert
Delete
Replace
```

---

# State

```cpp
dp[i][j]
```

Meaning:

```txt
Minimum operations required to convert

word1[0...i-1]

into

word2[0...j-1]
```

---

# Base Cases

## Empty Source

If source becomes empty:

```txt
""

↓

word2 prefix
```

Need to insert every character.

```cpp
dp[0][j] = j;
```

---

## Empty Target

If target becomes empty:

```txt
word1 prefix

↓

""
```

Need to delete every character.

```cpp
dp[i][0] = i;
```

---

# Transition

## Characters Match

```cpp
if(word1[i-1] == word2[j-1])
```

No operation is required.

```cpp
dp[i][j] = dp[i-1][j-1];
```

---

## Characters Don't Match

Three possible operations.

---

### Insert

Insert current target character.

```cpp
dp[i][j-1]
```

Reason:

```txt
Current source remains same.

Target prefix becomes smaller.
```

---

### Delete

Delete current source character.

```cpp
dp[i-1][j]
```

Reason:

```txt
Source prefix becomes smaller.

Target remains same.
```

---

### Replace

Replace current source character.

```cpp
dp[i-1][j-1]
```

Reason:

```txt
Current characters become equal.

Both prefixes become smaller.
```

---

## Final Transition

```cpp
if(word1[i-1] == word2[j-1])
    dp[i][j] = dp[i-1][j-1];
else
    dp[i][j] = 1 + min({
        dp[i][j-1],      // Insert
        dp[i-1][j],      // Delete
        dp[i-1][j-1]     // Replace
    });
```

---

# Why These Three Operations?

Suppose

```txt
horse

ros
```

At some point:

```txt
r != o
```

Choices:

```txt
Insert

Delete

Replace
```

Try all three.

Choose minimum.

---

# Tabulation

Fill DP from:

```txt
(0,0)

↓

(n,m)
```

using the same recurrence.

Answer:

```cpp
dp[n][m]
```

---

# Space Optimization (Two Arrays)

Observation:

```cpp
dp[i][j]
```

depends on:

```cpp
dp[i][j-1]

dp[i-1][j]

dp[i-1][j-1]
```

Only previous row and current row are needed.

---

## State Mapping

Before processing row:

```cpp
prev[j]
```

stores:

```txt
dp[i-1][j]
```

---

During processing:

```cpp
curr[j]
```

stores:

```txt
dp[i][j]
```

---

## Transition

```cpp
if(match)
    curr[j] = prev[j-1];
else
    curr[j] = 1 + min({
        curr[j-1],
        prev[j],
        prev[j-1]
    });
```

---

# One Array Optimization

The dependency graph is identical to **LCS**.

Need:

```cpp
dp[i-1][j]

dp[i][j-1]

dp[i-1][j-1]
```

Compress into one array using:

```cpp
prevDiag
```

---

## State Mapping

Before update:

| Variable   | Represents     |
| ---------- | -------------- |
| `dp[j]`    | `dp[i-1][j]`   |
| `dp[j-1]`  | `dp[i][j-1]`   |
| `prevDiag` | `dp[i-1][j-1]` |

---

## Initialization

```cpp
for(int j=0;j<=m;j++)
    dp[j]=j;
```

Before every row:

```cpp
int prevDiag = dp[0];

dp[0] = i;
```

---

## Correct 1D Transition

```cpp
int temp = dp[j];

if(word1[i-1] == word2[j-1]){
    dp[j] = prevDiag;
}
else{
    dp[j] = 1 + min({
        dp[j-1],      // Insert
        dp[j],        // Delete
        prevDiag      // Replace
    });
}

prevDiag = temp;
```

---

# Why Save `temp`?

Before updating:

```cpp
temp = dp[j];
```

stores:

```txt
dp[i-1][j]
```

After computing current cell:

```cpp
prevDiag = temp;
```

So for the next column:

```txt
prevDiag

=

dp[i-1][j]
```

which becomes:

```txt
dp[i-1][j-1]
```

for the next iteration.

---

# Common Mistake #1

Wrong base cases.

Remember:

```cpp
dp[0][j] = j;
dp[i][0] = i;
```

---

# Common Mistake #2

Wrong meaning of operations.

| Operation | Transition     |
| --------- | -------------- |
| Insert    | `dp[i][j-1]`   |
| Delete    | `dp[i-1][j]`   |
| Replace   | `dp[i-1][j-1]` |

Derive from the state instead of memorizing.

---

# Common Mistake #3

Forgetting:

```cpp
dp[0] = i;
```

during 1D optimization.

Otherwise first column remains incorrect.

---

# Common Mistake #4

Updating:

```cpp
prevDiag = dp[j];
```

after modifying `dp[j]`.

Wrong.

Need:

```cpp
int temp = dp[j];

...

prevDiag = temp;
```

because `prevDiag` should store the **old** value:

```txt
dp[i-1][j]
```

---

# Complexity

## Memoization

Time:

```txt
O(nm)
```

Space:

```txt
O(nm)
```

(+ recursion stack)

---

## Tabulation

Time:

```txt
O(nm)
```

Space:

```txt
O(nm)
```

---

## Two Arrays

Time:

```txt
O(nm)
```

Space:

```txt
O(m)
```

---

## One Array

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
Minimum operations

Transform one string into another

Insert / Delete / Replace
```

Think:

```txt
State:

dp[i][j]

=

Minimum operations to convert

word1 prefix

↓

word2 prefix

Match
↓

Diagonal

Mismatch
↓

Insert

Delete

Replace
```

---

# Related Problems

Uses the same **previous row + current row + previous diagonal** dependency:

- [https://leetcode.com/problems/longest-common-subsequence/](https://leetcode.com/problems/longest-common-subsequence/)
- [https://leetcode.com/problems/delete-operation-for-two-strings/](https://leetcode.com/problems/delete-operation-for-two-strings/)
- [https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/](https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/)

---

# Interview Trigger

```txt
If you see:

Minimum edits

Transform strings

Insert / Delete / Replace

Think:

Edit Distance

State:
dp[i][j]

=

Minimum operations

Transition:

Match
→ Diagonal

Mismatch
→
1 + min(
Insert,
Delete,
Replace
)

Space Optimization
→ prevDiag trick
```

---

# DP Compression Patterns (Updated)

| Dependency                                     | Iteration | Extra Variable | Problems                            |
| ---------------------------------------------- | --------- | -------------- | ----------------------------------- |
| Previous row only                              | Backward  | No             | 0/1 Knapsack, Distinct Subsequences |
| Current row only                               | Forward   | No             | Unbounded Knapsack, Coin Change II  |
| Previous row + Current row + Previous diagonal | Forward   | `prevDiag`     | LCS, LPS, Edit Distance             |

## Rule of Thumb

```txt
Only previous-row dependencies
→ Backward iteration

Current-row dependency
→ Forward iteration

Need previous-row + current-row + diagonal
→ Forward iteration + prevDiag
```

This dependency-based approach is much easier to derive during interviews than memorizing the iteration order for every individual problem.
