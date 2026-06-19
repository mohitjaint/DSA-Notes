# Wildcard Matching

## References

Question:

[https://leetcode.com/problems/wildcard-matching/](https://leetcode.com/problems/wildcard-matching/)

Related Problems:

[https://leetcode.com/problems/regular-expression-matching/](https://leetcode.com/problems/regular-expression-matching/)

[https://leetcode.com/problems/distinct-subsequences/](https://leetcode.com/problems/distinct-subsequences/)

[https://leetcode.com/problems/interleaving-string/](https://leetcode.com/problems/interleaving-string/)

---

# Pattern

```txt
DP on Strings

Pattern Matching

Suffix DP
```

---

# Problem Statement

Given:

```txt
String s

Pattern p
```

Pattern contains:

```txt
?

*

Normal characters
```

Rules:

```txt
?

Matches exactly one character.

*

Matches zero or more characters.
```

Determine whether the entire string matches the entire pattern.

---

# Key Observation

There are three possible pattern characters.

### Normal Character

Must match exactly.

---

### '?'

Matches exactly one character.

---

### '\*'

Can match:

```txt
0 characters

1 character

2 characters

...

Any number of characters
```

The important DP insight is:

```txt
Do NOT try every possible length.

Only two recursive choices are sufficient.
```

---

# DP State

```cpp
dp[i][j]
```

Meaning:

```txt
Can

p[i...]

match

s[j...] ?
```

Notice this is a **Suffix DP**, unlike LCS or Edit Distance.

---

# Base Cases

## Pattern Finished

```cpp
if(i == n)
```

Pattern can match only if string is also finished.

```cpp
return j == m;
```

---

## String Finished

```cpp
if(j == m)
```

Remaining pattern must contain only `'*'`.

Example:

```txt
Pattern = "***"

↓

Matches ""
```

But

```txt
"*a"

↓

Cannot match ""
```

---

# Transition

## Normal Character

If

```cpp
p[i] == s[j]
```

Move diagonally.

```cpp
solve(i+1,j+1)
```

---

## '?'

Consumes exactly one character.

```cpp
solve(i+1,j+1)
```

---

## '\*'

Two possibilities.

### Match Zero Characters

Skip the star.

```cpp
solve(i+1,j)
```

---

### Match One Character

Consume one character while keeping the star.

```cpp
solve(i,j+1)
```

Notice:

```txt
Matching two or more characters

is automatically handled

by repeatedly choosing

solve(i,j+1).
```

Example:

```txt
solve(i,j)

↓

solve(i,j+1)

↓

solve(i,j+2)

↓

solve(i,j+3)

↓

solve(i+1,j+3)
```

Thus only **two transitions** are required.

---

# Memoization Transition

```cpp
if(p[i]=='?' || p[i]==s[j])
    return solve(i+1,j+1);

else if(p[i]=='*')
    return solve(i+1,j)
        || solve(i,j+1);

else
    return false;
```

---

# Why No Loop?

A common first idea is:

```cpp
for(all possible lengths)
```

or

```cpp
while(...)
```

This is unnecessary.

Reason:

```txt
The transition

solve(i,j+1)

keeps the same '*'

allowing it to consume

another character later.
```

Hence all lengths are naturally explored.

---

# Tabulation

State remains identical.

```cpp
dp[i][j]
```

means

```txt
Can

p[i...]

match

s[j...] ?
```

Fill table from:

```txt
Bottom Right

↓

Top Left
```

because every state depends on suffixes.

---

# Base Cases

## Both Finished

```cpp
dp[n][m] = true;
```

---

## Empty String

Only suffixes containing all `'*'` can match.

```cpp
for(int i=n-1;i>=0;i--){

    if(p[i]!='*')
        break;

    dp[i][m]=true;
}
```

---

# Transition

### Match

```cpp
dp[i][j]
=
dp[i+1][j+1]
```

---

### '\*'

```cpp
dp[i][j]
=
dp[i+1][j]
||
dp[i][j+1]
```

---

### Otherwise

```cpp
false
```

---

# Space Optimization

Observation:

Need only

```txt
Current Row

Next Row

Next Diagonal
```

Therefore

```txt
One Array

+

nextDiag
```

---

# State Mapping

Before updating:

| Variable   | Represents     |
| ---------- | -------------- |
| `dp[j]`    | `dp[i+1][j]`   |
| `dp[j+1]`  | `dp[i][j+1]`   |
| `nextDiag` | `dp[i+1][j+1]` |

---

# Correct Initialization

Initially

```cpp
dp[m]=true;
```

Maintain whether the remaining suffix contains only `'*'`.

```cpp
bool assignTrue = true;
```

Before processing every row:

```cpp
if(p[i]!='*')
    assignTrue=false;
```

---

## Important Order

This is a very common mistake.

Correct:

```cpp
bool nextDiag = dp[m];

dp[m] = assignTrue;
```

Not

```cpp
dp[m] = assignTrue;

nextDiag = dp[m];
```

Reason:

```txt
nextDiag

must store

old dp[i+1][m]

before

dp[m]

is overwritten.
```

Exactly the same idea as `prevDiag` in LCS/Edit Distance.

---

# 1D Transition

```cpp
bool temp = dp[j];

if(p[i]=='?' || p[i]==s[j]){
    dp[j] = nextDiag;
}
else if(p[i]=='*'){
    dp[j] = dp[j] || dp[j+1];
}
else{
    dp[j] = false;
}

nextDiag = temp;
```

---

# Common Mistake #1

Wrong base case.

```cpp
if(j==m)
    return false;
```

Wrong.

Need to check whether the remaining pattern consists entirely of `'*'`.

---

# Common Mistake #2

Trying every length.

Wrong:

```cpp
while(...)
```

Correct:

```cpp
solve(i+1,j)

||

solve(i,j+1)
```

---

# Common Mistake #3

Wrong initialization of last column in 1D DP.

Wrong:

```cpp
dp[m] = assignTrue;

nextDiag = dp[m];
```

Correct:

```cpp
nextDiag = dp[m];

dp[m] = assignTrue;
```

Always save the old value before overwriting.

---

# Common Mistake #4

Wrong iteration order.

Since this is a **suffix DP**, iterate:

```txt
Bottom

↓

Top

Right

↓

Left
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

## Space Optimized

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
Wildcard

Pattern Matching

?

*

```

Think:

```txt
Suffix DP

State:

dp[i][j]

=

Can pattern suffix

match

string suffix?

'?' →

Diagonal

'*' →

Zero characters

OR

One character

Bottom-up filling

Bottom-right

↓

Top-left
```

---

# Interview Trigger

```txt
If you see:

Wildcard Matching

?

*

Think:

State:

Can

pattern suffix

match

string suffix?

Normal Character

↓

Diagonal

'?'

↓

Diagonal

'*'

↓

Skip Star

OR

Consume One Character

Space Optimization

↓

Current Row

Next Row

Next Diagonal

↓

1D DP + nextDiag
```

---

# DP Compression Patterns (Updated)

| Dependency                                         | Iteration | Extra Variable | Problems                            |
| -------------------------------------------------- | --------- | -------------- | ----------------------------------- |
| Previous row only                                  | Backward  | No             | 0/1 Knapsack, Distinct Subsequences |
| Current row only                                   | Forward   | No             | Unbounded Knapsack, Coin Change II  |
| Previous row + Current row + Previous diagonal     | Forward   | `prevDiag`     | LCS, LPS, Edit Distance             |
| Next row + Current row + Next diagonal (Suffix DP) | Backward  | `nextDiag`     | Wildcard Matching                   |

## Rule of Thumb

```txt
Prefix DP
↓

Usually fills

Top-left

↓

Bottom-right

Suffix DP
↓

Usually fills

Bottom-right

↓

Top-left

Need previous diagonal
↓

prevDiag

Need next diagonal
↓

nextDiag
```

This dependency-based classification is much easier to derive in interviews than memorizing individual problems or iteration orders.
