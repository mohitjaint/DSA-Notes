# Shortest Common Supersequence (SCS)

## References

Question:

[https://leetcode.com/problems/shortest-common-supersequence/](https://leetcode.com/problems/shortest-common-supersequence/)

Related Problems:

[https://leetcode.com/problems/longest-common-subsequence/](https://leetcode.com/problems/longest-common-subsequence/)

[https://www.geeksforgeeks.org/problems/print-longest-common-subsequence/1](https://www.geeksforgeeks.org/problems/print-longest-common-subsequence/1)

[https://leetcode.com/problems/delete-operation-for-two-strings/](https://leetcode.com/problems/delete-operation-for-two-strings/)

[https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/](https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/)

---

# Pattern

```txt
LCS Reconstruction

Shortest Common Supersequence

Backtracking on LCS DP
```

---

# Key Observation

A Supersequence:

```txt
Contains both strings
as subsequences.
```

To make it **shortest**:

```txt
Do not duplicate
common characters.
```

Therefore:

```txt
Find the LCS first.

Common characters
are written only once.
```

---

# Important Formula

```txt
Length(SCS)

=

Length(str1)
+
Length(str2)
-
Length(LCS)
```

---

## Example

```txt
str1 = abac
str2 = cab

LCS = ab

SCS Length

=
4 + 3 - 2

=
5
```

One valid answer:

```txt
cabac
```

---

# Why LCS?

Suppose

```txt
str1 = abc

str2 = adc
```

Common subsequence:

```txt
ac
```

Need not write:

```txt
a
c
```

twice.

Hence:

```txt
Common characters

↓

Take once
```

Everything else must appear.

---

# DP State

Compute normal LCS DP.

```cpp
dp[i][j]
```

Meaning:

```txt
Length of LCS

between

str1[0...i-1]

and

str2[0...j-1]
```

---

# Reconstruction

Start from:

```cpp
i = n

j = m
```

Build answer backwards.

---

## Case 1 : Characters Match

```cpp
if(str1[i-1] == str2[j-1])
```

Take character once.

```cpp
ans += str1[i-1];

i--;
j--;
```

Move diagonally.

---

## Case 2 : Characters Don't Match

Need to preserve maximum LCS.

---

### Move Up

If:

```cpp
dp[i-1][j] > dp[i][j-1]
```

Take:

```cpp
str1[i-1]
```

Move:

```txt
↑
```

```cpp
ans += str1[i-1];
i--;
```

---

### Move Left

Otherwise

Take:

```cpp
str2[j-1]
```

Move:

```txt
←
```

```cpp
ans += str2[j-1];
j--;
```

---

# Remaining Characters

If:

```txt
One string finishes
```

Append all remaining characters from the other string.

```cpp
while(i>0){
    ans += str1[i-1];
    i--;
}

while(j>0){
    ans += str2[j-1];
    j--;
}
```

---

# Final Step

Since reconstruction starts from the end:

```cpp
reverse(ans.begin(), ans.end());
```

---

# Why Does It Work?

Every common character:

```txt
Appears only once.
```

Every non-common character:

```txt
Must be included.
```

Following the larger LCS value guarantees:

```txt
Maximum sharing

↓

Minimum total length.
```

---

# Complete Reconstruction

```cpp
while(i>0 && j>0){

    if(str1[i-1] == str2[j-1]){
        ans += str1[i-1];
        i--;
        j--;
    }
    else if(dp[i-1][j] > dp[i][j-1]){
        ans += str1[i-1];
        i--;
    }
    else{
        ans += str2[j-1];
        j--;
    }
}

while(i>0){
    ans += str1[i-1];
    i--;
}

while(j>0){
    ans += str2[j-1];
    j--;
}

reverse(ans.begin(), ans.end());
```

---

# Common Mistake #1

Starting reconstruction from:

```cpp
i = n+1;

j = m+1;
```

Wrong.

Correct:

```cpp
i = n;

j = m;
```

Reason:

```txt
i and j

represent

prefix lengths,

not indices.
```

---

# Common Mistake #2

Appending:

```cpp
str1[i]
```

instead of:

```cpp
str1[i-1]
```

during reconstruction.

Remember:

```txt
Current character

=

index

i-1
```

---

# Common Mistake #3

Forgetting to append remaining characters.

Example:

```txt
abc

ab
```

After DP:

```txt
One string still has

c
```

Must append it.

---

# Common Mistake #4

Forgetting to reverse.

Backtracking builds:

```txt
End

↓

Beginning
```

Need:

```cpp
reverse(ans.begin(), ans.end());
```

---

# Complexity

### DP Construction

Time:

```txt
O(nm)
```

Space:

```txt
O(nm)
```

---

### Reconstruction

Time:

```txt
O(n+m)
```

---

### Overall

Time:

```txt
O(nm)
```

Space:

```txt
O(nm)
```

---

# Relation With LCS

| Problem                       | Reconstruction                                             |
| ----------------------------- | ---------------------------------------------------------- |
| Print One LCS                 | Take only matching characters                              |
| Shortest Common Supersequence | Take matching characters once + include skipped characters |
| Print All LCS                 | Branch into every optimal parent                           |

---

# Pattern Recognition

Whenever you see:

```txt
Shortest Common Supersequence

Two Strings

Minimum Combined String
```

Think:

```txt
Compute LCS

↓

Backtrack

↓

Match
→ Take once

Mismatch
→ Take skipped character
and move towards
larger LCS

↓

Reverse
```

---

# Interview Trigger

```txt
If you see:

Shortest Common Supersequence

Think:

LCS Reconstruction

Formula:

SCS Length

=

|A|
+
|B|
-
LCS

Backtracking Rules:

Match
→ Take once

Mismatch
→ Take character from
the direction having
larger LCS value

Append remaining characters

Reverse answer
```
