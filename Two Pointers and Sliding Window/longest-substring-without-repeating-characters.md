# Longest Substring Without Repeating Characters

## References

**Question:**

[https://leetcode.com/problems/longest-substring-without-repeating-characters/](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

**Related Problems:**

- Max Consecutive Ones III
- Longest Repeating Character Replacement
- Minimum Window Substring
- Fruit Into Baskets

---

# Pattern

```text
Sliding Window

Variable Size Window

Last Occurrence
```

---

# Problem Statement

Given a string,

find the length of the longest substring containing **no repeated characters**.

---

# Brute Force

Start every substring from every index.

Expand until a repeated character is found.

Use a hash set/map to detect duplicates.

---

## Complexity

```text
Time  : O(n²)

Space : O(Σ)
```

where

```text
Σ
```

is the character set size.

---

# Observation

A substring is valid if

```text
Every character appears only once.
```

Whenever a duplicate appears,

all characters before its **previous occurrence** become useless.

Instead of removing them one by one,

we can **jump** the left pointer directly.

---

# Sliding Window State

Maintain

```cpp
l
```

Left boundary of the window.

```cpp
r
```

Right boundary (expands every iteration).

```cpp
lastInd[ch]
```

Last index where character

```text
ch
```

appeared.

---

# Window Invariant

```text
Window

[l........r]

contains only unique characters.
```

---

# Transition

For every character

```cpp
s[r]
```

there are two possibilities.

---

## Case 1

Character has **not appeared** before.

Simply extend the window.

---

## Case 2

Character appeared before.

If its previous occurrence lies **inside** the current window,

move

```cpp
l
```

just after its previous occurrence.

```cpp
l = lastInd[s[r]] + 1;
```

---

# Why the Condition?

We only move

```cpp
l
```

if

```cpp
lastInd[s[r]] >= l
```

Suppose

```text
abba
```

Processing

```text
a
```

at index

```text
3
```

Previous occurrence

```text
0
```

Current window

```text
[2...3]
```

Since

```text
0 < l
```

the previous

```text
a
```

is already outside the window.

No need to move

```cpp
l
```

again.

Hence

```cpp
if(lastInd.count(s[r]) &&
   lastInd[s[r]] >= l)
```

---

# Update Last Occurrence

After processing

```cpp
s[r]
```

store its latest position.

```cpp
lastInd[s[r]] = r;
```

---

# Update Answer

```cpp
answer = max(answer, r-l+1);
```

---

# Complete Transition

```cpp
for(int r=0;r<n;r++){

    if(lastInd.count(s[r]) &&
       lastInd[s[r]]>=l){

        l=lastInd[s[r]]+1;
    }

    lastInd[s[r]]=r;

    ans=max(ans,r-l+1);
}
```

---

# Complexity

Each index is processed exactly once.

```text
Time : O(n)

Space : O(Σ)
```

---

# Why We Don't Use `while`

Unlike most sliding window problems,

we know **exactly** where the duplicate occurred.

Suppose

```text
abca
```

When processing the last

```text
a
```

we already know

```text
Previous a = index 0
```

Everything before

```text
0
```

is irrelevant.

Instead of removing

```text
a

↓

b

↓

c
```

one by one,

we directly jump

```cpp
l = 0 + 1;
```

This makes the update

```text
O(1)
```

instead of repeatedly shrinking.

---

# Why Does the Jump Work?

Suppose

```text
abcade
```

Current window

```text
abcade
```

Duplicate

```text
a
```

Previous occurrence

```text
0
```

Every valid substring must start **after** index

```text
0
```

Therefore

```cpp
l = 1;
```

No intermediate positions can produce a better answer.

---

# Common Mistake #1

Always writing

```cpp
l = lastInd[s[r]]+1;
```

Wrong.

Need

```cpp
lastInd[s[r]] >= l
```

Otherwise,

the duplicate may already be outside the current window.

---

# Common Mistake #2

Updating

```cpp
lastInd
```

before checking the duplicate.

Wrong

```cpp
lastInd[s[r]]=r;

if(...)
```

Correct

```cpp
if(...)

lastInd[s[r]]=r;
```

Otherwise,

you overwrite the previous occurrence.

---

# Common Mistake #3

Using a frequency map with

```cpp
while(freq>1)
```

Although correct,

it performs unnecessary shrinking.

Knowing the **last occurrence** allows a direct jump.

---

# Pattern Recognition

Whenever the problem says

```text
Longest Substring

↓

No repeated characters
```

think

```text
Sliding Window

+

Last Occurrence Map
```

instead of

```text
Frequency Map
```

---

# Comparison

| Frequency Map                   | Last Occurrence                        |
| ------------------------------- | -------------------------------------- |
| Shrinks one character at a time | Jumps directly                         |
| Uses `while`                    | Uses `if`                              |
| O(n)                            | O(n)                                   |
| Generic template                | More optimized for uniqueness problems |

---

# Interview Trigger

```text
Longest Substring

↓

Unique Characters

↓

Need previous occurrence

↓

Store last index

↓

Duplicate inside window?

↓

Jump left pointer

↓

Update last index

↓

Update answer

↓

O(n)
```

---

# Core Insight

The window becomes invalid only because of the **current character**. Since we know exactly where this character last appeared, we can immediately move the left pointer to one position after that occurrence instead of shrinking the window one character at a time. This transforms the problem into a simple **Sliding Window + Last Occurrence Map** pattern, achieving an optimal **O(n)** solution.
