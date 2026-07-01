# Palindrome Partitioning II

## References

**Question:**

[https://leetcode.com/problems/palindrome-partitioning-ii/](https://leetcode.com/problems/palindrome-partitioning-ii/)

**Related Problems:**

* Matrix Chain Multiplication
* Minimum Cost to Cut a Stick
* Burst Balloons
* Boolean Parenthesization
* Palindrome Partitioning I

---

# Pattern

```text
Partition DP

State Optimization

Palindrome DP
```

---

# Problem Statement

Given a string,

partition it into substrings such that

every substring is a palindrome.

Return the **minimum number of cuts** required.

---

# Initial Observation

At first glance,

it looks exactly like MCM.

```text
Choose a cut

↓

Left Substring

+

Right Substring
```

This leads to the recurrence

```cpp
f(i,j)

=

1 + f(i,k) + f(k+1,j)
```

This solution is correct,

but not optimal.

---

# Why the 2D State is Unnecessary

Suppose

```text
aabcb
```

Optimal partition

```text
aa | bcb
```

Notice

```text
aa
```

is already a palindrome.

Therefore

```text
f("aa") = 0
```

The left side never needs further partitioning.

---

# Core Observation

The first partition should **always** be a palindrome.

If

```text
aab | ...
```

is not a palindrome,

it must eventually become

```text
aa | b | ...
```

So directly choosing

```text
aa | ...
```

is always better.

Hence,

only consider palindrome prefixes.

---

# State Optimization

Instead of

```cpp
dp[i][j]
```

define

```cpp
dp[i]
```

Meaning

```text
Minimum number of partitions

needed for

suffix starting at index i.
```

---

# DP State

```cpp
dp[i]
```

Meaning

```text
Minimum partitions

required for

s[i...n-1]
```

Notice

we count

```text
Partitions

NOT

Cuts.
```

At the end,

```cpp
answer = dp[0]-1;
```

---

# Base Case

```cpp
dp[n]=0;
```

Meaning

```text
Empty suffix

needs

0 partitions.
```

---

# Transition

Try every palindrome prefix.

```text
i........k
```

If

```text
s[i...k]
```

is a palindrome,

then

```cpp
dp[i]

=

1 + dp[k+1]
```

Take the minimum.

Transition

```cpp
for(int k=i;k<n;k++){

    if(palindrome(i,k)){

        dp[i]=min(
            dp[i],
            1+dp[k+1]
        );
    }
}
```

---

# Why Subtract One?

Suppose

```text
aab
```

Optimal partition

```text
aa | b
```

Partitions

```text
2
```

Cuts

```text
1
```

Relation

```text
Cuts

=

Partitions - 1
```

Hence

```cpp
return dp[0]-1;
```

---

# Complexity (Without Optimization)

Palindrome checking

```cpp
isPalindrome(i,k)
```

takes

```text
O(n)
```

For every state

```text
States

O(n)
```

Transitions

```text
O(n)
```

Total

```text
O(n³)
```

---

# Final Optimization

Notice

```cpp
isPalindrome(i,k)
```

is computed repeatedly.

Precompute

```cpp
pal[i][j]
```

---

# Palindrome DP

State

```cpp
pal[i][j]
```

Meaning

```text
Whether

s[i...j]

is a palindrome.
```

---

# Transition

```cpp
pal[i][j]

=

(s[i]==s[j])

&&

(j-i<2 || pal[i+1][j-1])
```

Reason

For a palindrome

```text
Ends must match

AND

Middle must also be a palindrome.
```

---

# Palindrome DP Iteration

Dependency

```text
pal[i][j]

↓

pal[i+1][j-1]
```

Therefore

```cpp
for(int i=n-1;i>=0;i--){

    for(int j=i;j<n;j++){

        ...
    }
}
```

---

# Final DP

```cpp
for(int i=n-1;i>=0;i--){

    int mini=INF;

    for(int k=i;k<n;k++){

        if(pal[i][k]){

            mini=min(
                mini,
                1+dp[k+1]
            );
        }
    }

    dp[i]=mini;
}
```

---

# Complexity

Palindrome DP

```text
O(n²)
```

Main DP

```text
O(n²)
```

Overall

```text
Time  : O(n²)

Space : O(n²)
```

---

# Evolution of the Solution

### Approach 1

```text
Partition DP

State

dp[i][j]

↓

O(n⁴)
```

---

### Observation

Left partition must already be a palindrome.

↓

State reduces.

---

### Approach 2

```text
Suffix DP

State

dp[i]

↓

O(n³)
```

---

### Observation

Palindrome checking is repeated.

↓

Precompute palindromes.

---

### Approach 3 (Optimal)

```text
Palindrome DP

+

Suffix DP

↓

O(n²)
```

---

# Common Mistake #1

Using

```cpp
dp[i][j]
```

The left side never needs further partitioning.

Use

```cpp
dp[i]
```

instead.

---

# Common Mistake #2

Returning

```cpp
dp[0]
```

instead of

```cpp
dp[0]-1
```

DP counts partitions,

not cuts.

---

# Common Mistake #3

Checking palindrome every time.

```cpp
isPalindrome(i,j)
```

inside the transition makes the solution

```text
O(n³)
```

Precompute

```cpp
pal[i][j]
```

instead.

---

# Common Mistake #4

Using

```cpp
dp[n-1]
```

as the base case.

Correct base case

```cpp
dp[n]=0;
```

representing the empty suffix.

---

# Common Mistake #5

Thinking this is a standard Partition DP.

Initially it is,

but after the observation

```text
First partition must already be a palindrome.
```

the interval state collapses into a **1D suffix DP**.

---

# Pattern Recognition

When a problem says

```text
Partition a string

↓

Each partition must satisfy a property

↓

Optimize the number of partitions/cuts
```

ask

```text
Can the first partition already satisfy the property?

↓

If yes,

only recurse on the remaining suffix.
```

This often reduces

```text
2D DP

↓

1D DP
```

---

# Interview Trigger

```text
Palindrome Partitioning II

↓

Looks like Partition DP

↓

Observation

First piece must already be a palindrome

↓

State

dp[i]

↓

Need fast palindrome queries

↓

Precompute

pal[i][j]

↓

Transition

1 + dp[k+1]

↓

Answer

dp[0]-1

↓

O(n²)
```

---

# Core Insight

The key breakthrough is recognizing that the **left partition never needs further recursion**—it must already be a palindrome. This eliminates one dimension of the DP state, reducing the problem from an interval DP (`dp[i][j]`) to a suffix DP (`dp[i]`). The remaining bottleneck is repeated palindrome checking, which is removed by precomputing a palindrome table (`pal[i][j]`). This combination of **state reduction** and **preprocessing DP** transforms an apparent `O(n⁴)` Partition DP into an optimal `O(n²)` solution.
