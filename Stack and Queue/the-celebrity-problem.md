# Find the Celebrity (GFG)

## References

**Question:**

[https://www.geeksforgeeks.org/problems/the-celebrity-problem/1](https://www.geeksforgeeks.org/problems/the-celebrity-problem/1)

**Related Problems:**

- Find the Town Judge (LeetCode 997)
- Gas Station (Candidate Elimination)
- Majority Element (Boyer-Moore Voting)

---

# Pattern

```text
Candidate Elimination

Two Pointers

Verification
```

---

# Problem Statement

Given an `N × N` matrix

```text
mat[i][j]
```

where

```text
1 → i knows j

0 → i does not know j
```

find the celebrity.

A celebrity

- knows nobody
- is known by everyone

> **GFG Convention:** `mat[i][i] = 1` (everyone knows themselves).

---

# Brute Force

For every person,

count

- how many people they know
- how many people know them

If

```text
Knows only themselves

AND

Everyone knows them
```

return that person.

---

## Complexity

```text
Time  : O(n²)

Space : O(n)
```

---

# Key Observation

Suppose there are two people

```text
A      B
```

If

```text
A knows B
```

then

```text
A cannot be celebrity.
```

If

```text
A does NOT know B
```

then

```text
B cannot be celebrity.
```

A single comparison eliminates at least one candidate.

---

# Candidate Elimination

Maintain

```text
top = 0

bottom = n-1
```

While

```text
top < bottom
```

---

## Case 1

```cpp
mat[top][bottom] == 1
```

```text
top knows bottom

↓

top cannot be celebrity

↓

top++
```

---

## Case 2

```cpp
mat[top][bottom] == 0

AND

mat[bottom][top] == 1
```

```text
bottom knows top

↓

bottom cannot be celebrity

↓

bottom--
```

---

## Case 3

```cpp
mat[top][bottom] == 0

AND

mat[bottom][top] == 0
```

Neither knows the other.

```text
top cannot be celebrity

(bottom doesn't know top)

↓

bottom cannot be celebrity

(bottom knows nobody)

↓

Discard both

↓

top++

bottom--
```

---

# After Elimination

Only one candidate remains.

Verify it.

For GFG

```text
Celebrity knows only themselves

↓

Outgoing edges = 1

Everyone (including themselves) knows celebrity

↓

Incoming edges = n
```

---

# Verification

```cpp
for(int i = 0; i < n; i++){
    if(mat[candidate][i]) iKnow++;
    if(mat[i][candidate]) knowsMe++;
}
```

Check

```cpp
iKnow == 1

&&

knowsMe == n
```

---

# Algorithm

```text
Two pointers

↓

Compare top and bottom

↓

Discard one (or both)

↓

One candidate remains

↓

Verify candidate

↓

Return answer
```

---

# Example

```text
      0 1 2

0 -> 1 1 0

1 -> 0 1 0

2 -> 1 1 1
```

Start

```text
top = 0

bottom = 2
```

Since

```text
0 does not know 2
```

Person

```text
2
```

cannot be celebrity.

Discard

```text
2
```

Now

```text
top = 0

bottom = 1
```

Since

```text
0 knows 1
```

Discard

```text
0
```

Candidate

```text
1
```

Verify

```text
Knows only self

Everyone knows 1
```

Answer

```text
1
```

---

# Why It Works

Every comparison removes at least one impossible candidate.

Initially

```text
n candidates
```

After one comparison

```text
n-1 candidates
```

Eventually

```text
1 candidate
```

Only that person needs verification.

---

# Complexity

| Approach               | Time  | Space |
| ---------------------- | ----- | ----: |
| Counting In/Out Degree | O(n²) |  O(n) |
| Candidate Elimination  | O(n)  |  O(1) |

---

# Common Mistake #1

Using the wrong verification condition.

For **GFG**

```cpp
iKnow == 1

knowsMe == n
```

because

```text
mat[i][i] = 1
```

For **LeetCode / Standard Definition**

```cpp
iKnow == 0

knowsMe == n-1
```

because

```text
mat[i][i] = 0
```

Always check the problem's convention.

---

# Common Mistake #2

Returning the remaining candidate without verification.

Candidate elimination only guarantees

```text
At most one candidate.
```

It does **not** guarantee that a celebrity actually exists.

Always verify.

---

# Common Mistake #3

Using an `O(n²)` scan after already finding the candidate.

Only the remaining candidate needs verification.

Verification is

```text
O(n)
```

---

# Comparison

| Problem           | Pattern                             |
| ----------------- | ----------------------------------- |
| Celebrity Problem | Candidate Elimination               |
| Town Judge        | Degree Counting                     |
| Majority Element  | Candidate Elimination (Boyer-Moore) |
| Gas Station       | Candidate Elimination               |

---

# Pattern Recognition

Whenever you see

```text
Exactly one possible answer

↓

A comparison can eliminate one candidate

↓

Need final verification
```

think

```text
Candidate Elimination
```

This pattern reduces the search space without checking every possibility.

---

# Interview Trigger

```text
Need celebrity

↓

Compare two candidates

↓

One comparison eliminates at least one

↓

Repeat until one candidate remains

↓

Verify candidate

↓

O(n) Time

O(1) Space
```

---

# Core Insight

The crucial observation is that **one comparison is enough to eliminate at least one person**. If person `A` knows person `B`, then `A` cannot be the celebrity. Otherwise, `B` cannot be the celebrity. By repeatedly eliminating candidates, only one possible celebrity remains. A final `O(n)` verification confirms whether this candidate truly satisfies the celebrity conditions, giving an optimal **O(n)** solution.
