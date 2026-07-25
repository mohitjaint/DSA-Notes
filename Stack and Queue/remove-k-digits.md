# Remove K Digits (LeetCode 402)

## References

**Question:**

[https://leetcode.com/problems/remove-k-digits/](https://leetcode.com/problems/remove-k-digits/)

**Related Problems:**

- Remove Duplicate Letters
- Create Maximum Number
- Remove All Adjacent Duplicates
- Asteroid Collision (Stack Simulation)

---

# Pattern

```text
Greedy

Stack

Lexicographically Smallest Number
```

---

# Problem Statement

Given a non-negative integer represented as a string,

remove exactly

```text
k
```

digits so that the remaining number is **minimum possible**.

---

# Brute Force

Try every possible combination of removing

```text
k
```

digits.

Choose the smallest remaining number.

---

## Complexity

```text
Time : Exponential

Space : O(n)
```

Impossible for large inputs.

---

# Key Observation

The **leftmost digit** contributes the most to the value of a number.

Example

```text
5234
```

Removing

```text
5
```

gives

```text
234
```

Removing

```text
4
```

gives

```text
523
```

Clearly

```text
234 < 523
```

Therefore,

whenever a larger digit appears before a smaller digit,

the larger digit should be removed.

---

# Core Greedy Idea

Suppose we have

```text
54
```

Since

```text
5 > 4
```

keeping

```text
5
```

can never lead to the smallest possible answer.

Delete

```text
5
```

immediately.

---

# Why a Stack?

The most recently chosen digit is the only one that can become invalid.

Suppose

```text
1432
```

Current answer

```text
143
```

New digit

```text
2
```

Now

```text
3 > 2
```

Remove

```text
3
```

Current answer

```text
14
```

Again

```text
4 > 2
```

Remove

```text
4
```

Current answer

```text
1
```

Finally

```text
2
```

is appended.

Only the **last chosen digit** is compared repeatedly.

This is exactly LIFO behavior.

---

# Algorithm

For every digit

```text
While

k > 0

AND

stack not empty

AND

top > current digit

↓

Pop stack

↓

k--

---------------------

Skip leading zeroes

↓

Push current digit
```

---

# Why While?

Suppose

```text
765432
```

When

```text
5
```

arrives,

both

```text
7

6
```

should be removed if possible.

Using

```cpp
if(...)
```

would remove only one digit.

Hence

```cpp
while(...)
```

is necessary.

---

# Handling Leading Zeroes

Suppose

```text
10200
```

Remove

```text
1
```

Current answer becomes

```text
""
```

Next digit

```text
0
```

would become a leading zero.

Simply skip it.

```cpp
if(num[i]=='0' && st.empty())
    continue;
```

No extra cleanup is needed later.

---

# Remaining Deletions

Example

```text
123456

k = 2
```

The stack never pops because digits are increasing.

Remove from the end.

```cpp
while(k > 0){
    st.pop_back();
    k--;
}
```

---

# Empty Answer

Example

```text
10

k = 2
```

Everything is removed.

Return

```text
"0"
```

instead of an empty string.

---

# Example

```text
num

1432219

k = 3
```

Process

```text
1

↓

1

4

↓

14

3

↓

4 > 3

Remove 4

↓

13

2

↓

3 > 2

Remove 3

↓

12

2

↓

122

1

↓

2 > 1

Remove 2

↓

121

9

↓

1219
```

Answer

```text
1219
```

---

# Why It Works

Whenever

```text
previous digit > current digit
```

keeping the previous digit always makes the number larger.

Removing it immediately is always optimal because earlier digits have higher place value.

Thus,

every deletion is locally optimal and leads to the globally minimum number.

---

# Complexity

```text
Time : O(n)

Space : O(n)
```

Each digit is

- pushed once
- popped at most once.

---

# Common Mistake #1

Using

```cpp
if(...)
```

instead of

```cpp
while(...)
```

Multiple previous digits may need to be removed.

---

# Common Mistake #2

Forgetting leftover

```text
k
```

If digits are increasing,

nothing is removed during traversal.

Remove from the end.

---

# Common Mistake #3

Removing leading zeroes afterwards.

Cleaner approach:

```cpp
if(num[i]=='0' && st.empty())
    continue;
```

Never insert them.

---

# Common Mistake #4

Using a stack of indices.

The stack should contain the **current answer**, not indices to delete later.

Using a `string` as a stack is simpler:

```cpp
string st;

st.push_back()

st.pop_back()

st.back()
```

---

# Common Mistake #5

Returning an empty string.

If every digit is removed,

return

```text
"0"
```

---

# Comparison

| Problem                  | Pattern          | Stack Represents    |
| ------------------------ | ---------------- | ------------------- |
| Next Greater Element     | Monotonic Stack  | Candidate answers   |
| Sum of Subarray Minimums | Monotonic Stack  | Boundary indices    |
| Asteroid Collision       | Stack Simulation | Surviving asteroids |
| Remove K Digits          | Greedy + Stack   | Current best answer |

---

# Pattern Recognition

Whenever you see

```text
Build the smallest / largest answer

↓

Characters or digits arrive sequentially

↓

A previous choice may become suboptimal

↓

Undo the last choice
```

think

```text
Greedy

+

Stack
```

---

# Interview Trigger

```text
Need smallest number

↓

Earlier digits matter more

↓

If previous digit > current digit

↓

Delete previous digit

↓

Repeat while beneficial

↓

Push current digit

↓

Remove remaining digits from end

↓

Skip leading zeroes

↓

O(n)
```

---

# Core Insight

The stack always represents the **best number constructed so far**. When a smaller digit arrives, any larger digit immediately before it should be removed because replacing a more significant digit with a smaller one always yields a smaller overall number. The stack allows these greedy corrections to be made efficiently, with each digit pushed and popped at most once, resulting in an **O(n)** solution.
