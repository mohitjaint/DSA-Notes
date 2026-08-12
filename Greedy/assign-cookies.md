# Assign Cookies — LeetCode 455

## Reference

**Question:** [https://leetcode.com/problems/assign-cookies/](https://leetcode.com/problems/assign-cookies/)

**Pattern:**
`Greedy + Sorting + Two Pointers`

---

## Problem

We have:

- `g[i]` = greed factor of child `i`
- `s[j]` = size of cookie `j`

A child is satisfied if:

```text
cookie size >= child's greed factor
```

Each cookie can be given to at most one child.

**Goal:** maximize the number of satisfied children.

---

# Key Observation

The important greedy decision is:

> **Always try to satisfy the least greedy child using the smallest cookie that can satisfy them.**

Why?

Because using a larger cookie on an easy-to-satisfy child wastes a resource that could be needed by a greedier child.

---

# Step 1 — Sort

```cpp
sort(g.begin(), g.end());
sort(s.begin(), s.end());
```

Now both arrays are in increasing order.

Example:

```text
g = [1, 2, 3]
s = [1, 1, 2, 3]
```

---

# Step 2 — Two Pointers

```text
i → current child
j → current cookie
```

Initially:

```text
i = 0
j = 0
```

---

## Case 1: Cookie satisfies child

If:

```cpp
g[i] <= s[j]
```

then assign the cookie.

```text
child satisfied
    ↓
answer++
    ↓
move to next child
    ↓
move to next cookie
```

```cpp
i++;
j++;
```

---

## Case 2: Cookie is too small

If:

```cpp
g[i] > s[j]
```

then this cookie cannot satisfy the current child.

Because `g` is sorted, every remaining child is **at least as greedy** as the current child.

Therefore this cookie cannot satisfy any remaining child.

Discard it:

```cpp
j++;
```

---

# Example

```text
g = [1, 2, 3]
s = [1, 1, 2, 3]
```

### Child 1

```text
g[i] = 1
s[j] = 1
```

Satisfied.

```text
answer = 1
```

---

### Child 2

```text
g[i] = 2
s[j] = 1
```

Cookie too small.

Discard it.

```text
j++
```

---

### Child 2 again

```text
g[i] = 2
s[j] = 2
```

Satisfied.

```text
answer = 2
```

---

### Child 3

```text
g[i] = 3
s[j] = 3
```

Satisfied.

```text
answer = 3
```

Final answer:

```text
3
```

---

# Why the Greedy Choice Works

Consider the least greedy remaining child.

Suppose:

```text
child = 2

available cookies = [3, 5, 8]
```

Using `3` is always at least as good as using `5` or `8`.

Why?

```text
3 satisfies the child
```

while preserving:

```text
5, 8
```

for children who might require larger cookies.

So:

> **Use the smallest sufficient resource for the easiest remaining requirement.**

---

# Algorithm

```text
Sort children by greed

Sort cookies by size

i = 0
j = 0

while both arrays have elements:

    if cookie satisfies child:
        answer++
        i++
        j++

    else:
        discard cookie
        j++

return answer
```

---

# Code

```cpp
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());

        int i = 0, j = 0;
        int ans = 0;

        while(i < g.size() && j < s.size()) {
            if(g[i] <= s[j]) {
                ans++;
                i++;
                j++;
            }
            else {
                j++;
            }
        }

        return ans;
    }
};
```

---

# Complexity

Let:

```text
n = number of children
m = number of cookies
```

Sorting:

```text
O(n log n + m log m)
```

Two pointers:

```text
O(n + m)
```

Overall:

```text
Time  : O(n log n + m log m)
Space : O(1) extra
```

---

# Common Mistake

### Using the largest cookie for the easiest child

For example:

```text
g = [1, 2, 3]
s = [1, 2, 3]
```

Giving `3` to the child requiring `1` is wasteful.

Instead:

```text
1 → 1
2 → 2
3 → 3
```

The smallest sufficient cookie should always be used.

---

# Pattern Recognition

When you see:

```text
Requirements

+

Resources

+

Each resource can be used once

+

Maximize number of satisfied requirements
```

think:

```text
Sort both

↓

Two pointers

↓

Smallest sufficient resource
```

Common examples include:

- Assign Cookies
- Boats to Save People
- Assigning jobs/resources
- Interval scheduling variants

---

# Interview Trigger

```text
Resource allocation problem
        ↓
Need maximum matches
        ↓
Sort requirements
        ↓
Sort resources
        ↓
Take smallest requirement
        ↓
Smallest resource that satisfies it
        ↓
If resource too small → discard it
        ↓
Two pointers
```

---

# Core Insight

The greedy strategy is to **match the least demanding child with the smallest cookie that can satisfy them**. If the smallest available cookie cannot satisfy the current child, it cannot satisfy any remaining child either, so it can safely be discarded. Sorting makes these decisions possible with two pointers and gives an optimal **O(n log n + m log m)** solution.
