# Longest String Chain

## References

**Question:**

[https://leetcode.com/problems/longest-string-chain/](https://leetcode.com/problems/longest-string-chain/)

**Related Problems:**

- Longest Increasing Subsequence
- Largest Divisible Subset
- Print Longest Increasing Subsequence
- Word Ladder (different pattern)

---

# Pattern

```text
LIS Variation

Chain DP

State Redesign

HashMap DP
```

---

# Problem Statement

Given a list of words, return the length of the **longest possible word chain**.

A word `A` is a predecessor of word `B` if

- `B` has exactly one extra character.
- Removing that character from `B` produces `A`.

Example

```text
a

↓

ba

↓

bda

↓

bdca
```

Answer

```text
4
```

---

# Key Observation

Unlike LIS,

the ordering is **not** based on value.

Instead,

```text
A can come before B

iff

A is a predecessor of B.
```

To ensure every predecessor appears before its successor,

sort by

```cpp
word.length()
```

---

# Approach 1 : LIS-style DP

## DP State

```cpp
dp[i]
```

Meaning

```text
Longest String Chain

ending at

words[i]
```

---

## Sorting

```cpp
sort(words.begin(), words.end(),
[](const string &a,const string &b){
    return a.size()<b.size();
});
```

---

## Checking Predecessor

Instead of LCS,

use two pointers.

```cpp
bool isPredecessor(string &a,string &b){

    if(b.size()!=a.size()+1)
        return false;

    int i=0,j=0;

    while(i<a.size() && j<b.size()){

        if(a[i]==b[j]){
            i++;
            j++;
        }
        else{
            j++;
        }
    }

    return i==a.size();
}
```

---

## Transition

```cpp
for(int i=0;i<n;i++){

    dp[i]=1;

    for(int j=0;j<i;j++){

        if(isPredecessor(words[j],words[i])){

            dp[i]=max(dp[i],dp[j]+1);
        }
    }
}
```

---

## Answer

```cpp
*max_element(dp.begin(),dp.end())
```

---

## Complexity

Sorting

```text
O(N log N)
```

DP

```text
O(N² × L)
```

where

```text
L = maximum word length
```

Space

```text
O(N)
```

---

# Why Not Use LCS?

A predecessor satisfies

```text
Length Difference = 1

AND

LCS = smaller word length
```

This condition is correct,

but computing LCS for every pair costs

```text
O(L²)
```

making the total complexity

```text
O(N²L²)
```

Using a two-pointer predecessor check reduces this to

```text
O(N²L)
```

---

# Approach 2 : HashMap DP (Optimal)

## Key Observation

Instead of checking

```text
Current word

against

every previous word
```

generate

```text
All possible predecessors

of the current word.
```

Each predecessor is obtained by deleting one character.

Example

```text
abcd
```

Possible predecessors

```text
abc

abd

acd

bcd
```

Only

```text
Length(word)
```

possibilities exist.

---

## DP State

```cpp
unordered_map<string,int> dp;
```

Meaning

```text
dp[word]

=

Longest String Chain

ending at

word.
```

---

## Initialization

Process words in increasing order of length.

Initially

```cpp
dp[word]=1;
```

Every word alone forms a chain.

---

## Transition

Generate every predecessor.

```cpp
for(int i=0;i<word.size();i++){

    string pred=word;

    pred.erase(i,1);

    if(dp.count(pred)){

        dp[word]=max(
            dp[word],
            dp[pred]+1
        );
    }
}
```

---

## Answer

Maintain

```cpp
ans=max(ans,dp[word]);
```

during processing.

---

## Complexity

Sorting

```text
O(N log N)
```

Generating predecessors

```text
L
```

Each erase

```text
O(L)
```

Total

```text
O(NL²)
```

Space

```text
O(N)
```

---

# Why is HashMap Faster?

LIS-style DP

```text
For every word

↓

Compare with

every previous word

↓

O(N²)
```

HashMap DP

```text
For every word

↓

Generate only

valid predecessors

↓

Hash lookup

↓

O(L)
```

Instead of searching through every previous state,

we directly generate all possible previous states.

---

# Can We Use the O(n log n) LIS Optimization?

**No.**

The binary search optimization for LIS relies on maintaining

```text
Smallest possible tail

for every subsequence length.
```

A smaller tail is always better.

Example

```text
Tail = 3

is always better than

Tail = 5
```

because every element extending

```text
5
```

can also extend

```text
3
```

---

For String Chain,

there is no similar property.

Example

```text
abc

abd
```

Neither word is a universally better representative.

Future words may extend one but not the other.

Therefore

- no monotonic ordering exists,
- no "best tail" exists,
- binary search optimization is impossible.

---

# Common Mistake #1

Using LCS to check predecessors.

Correct but slower.

Prefer the two-pointer check.

---

# Common Mistake #2

Not sorting by word length.

Without sorting,

a predecessor may appear after its successor.

---

# Common Mistake #3

Checking

```text
Length Difference ≥ 1
```

instead of

```text
Exactly 1
```

Only one extra character is allowed.

---

# Common Mistake #4

Using LIS-style DP when HashMap DP is available.

The HashMap solution is both simpler and faster.

---

# Pattern Recognition

Whenever a DP problem asks

```text
Longest Chain
```

ask yourself

```text
Can I generate

all previous valid states

instead of searching

through every previous state?
```

If yes,

a HashMap DP often reduces

```text
O(N²)

↓

O(N × Number of Predecessors)
```

---

# Interview Trigger

```text
Longest String Chain

↓

Sort by Length

↓

Method 1

LIS-style DP

↓

dp[i]

=

Longest chain ending at i

↓

Check predecessor

↓

O(N²L)

OR

↓

Method 2

Generate predecessors

↓

HashMap DP

↓

dp[word]

=

Longest chain ending at word

↓

Delete one character

↓

Lookup predecessor

↓

O(NL²)
```

---

# Core Insight

This problem is another variation of the **LIS pattern**, but the optimal solution comes from changing **how predecessors are found**. Instead of comparing the current word with every previous word, observe that each word has only **`L` possible predecessors** (obtained by deleting one character). By generating these predecessors and storing DP values in an `unordered_map`, the search over all previous states is replaced with constant-time lookups, reducing the complexity from **O(N²L)** to **O(NL²)**.
