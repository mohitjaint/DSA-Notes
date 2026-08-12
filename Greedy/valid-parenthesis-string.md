# Valid Parenthesis String — LeetCode 678

## Reference

**Question:** [https://leetcode.com/problems/valid-parenthesis-string/](https://leetcode.com/problems/valid-parenthesis-string/)

**Pattern:**
`Greedy + Range of Possible Balances`

---

# Problem

The string contains:

```text
(
)
*
```

where `*` can represent:

```text
(
)
or empty string
```

Determine whether the string can be converted into a valid parenthesis string.

---

# Key Observation

For normal parentheses, we need:

```text
balance >= 0
```

for **every prefix**, and finally:

```text
balance == 0
```

The difficulty is that `*` can have three different meanings.

Instead of deciding immediately what every `*` represents, maintain a **range of possible balances**.

---

# Two Variables

```cpp
int minOpen = 0;
int maxOpen = 0;
```

### `minOpen`

The **minimum possible** number of unmatched opening parentheses after the current prefix.

### `maxOpen`

The **maximum possible** number of unmatched opening parentheses after the current prefix.

So the actual possible balance lies somewhere in:

```text
[minOpen, maxOpen]
```

---

# Processing `'('`

An opening parenthesis always increases the balance.

```cpp
minOpen++;
maxOpen++;
```

Example:

```text
Before: [2, 4]

'('

After:  [3, 5]
```

---

# Processing `')'`

A closing parenthesis always decreases the balance.

```cpp
minOpen--;
maxOpen--;
```

Example:

```text
Before: [2, 4]

')'

After: [1, 3]
```

---

# Processing `'*'`

`*` has three possibilities:

```text
'*' → '('
'*' → ')'
'*' → ''
```

Therefore:

```text
Minimum balance:
    treat '*' as ')'

Maximum balance:
    treat '*' as '('
```

So:

```cpp
minOpen--;
maxOpen++;
```

Example:

```text
Before: [2, 4]

'*'

After: [1, 5]
```

The empty-string interpretation lies between these possibilities.

---

# Important Condition

After processing each character:

```cpp
if(maxOpen < 0)
    return false;
```

Why?

`maxOpen` represents the **most optimistic possible balance**.

If even that is negative:

```text
maxOpen < 0
```

then every possible interpretation has more `)` than `(` in this prefix.

Therefore, the string can never become valid.

---

# Why Clamp `minOpen` to Zero?

```cpp
minOpen = max(minOpen, 0);
```

A negative `minOpen` simply means that our **minimum interpretation** has too many closing parentheses.

But we don't have to choose that interpretation.

For example:

```text
s = "*"
```

Initially:

```text
minOpen = 0
maxOpen = 0
```

After `*`:

```text
minOpen = -1
maxOpen = 1
```

The `-1` possibility corresponds to:

```text
*
↓
)
```

which is invalid.

But we can choose:

```text
*
↓
empty
```

or

```text
*
↓
(
```

So the useful minimum balance is:

```text
0
```

Hence:

```cpp
minOpen = max(minOpen, 0);
```

---

# Final Condition

After processing the entire string:

```cpp
return minOpen == 0;
```

Why?

`minOpen == 0` means:

> There exists at least one valid interpretation of all `*` characters that results in exactly zero unmatched opening parentheses.

If:

```text
minOpen > 0
```

then even the minimum possible balance still has unmatched `(`, so the string cannot be valid.

---

# Example

Consider:

```text
s = "(*))"
```

### Start

```text
[minOpen, maxOpen] = [0, 0]
```

### `'('`

```text
[1, 1]
```

### `'*'`

```text
[0, 2]
```

### `')'`

```text
[-1, 1]
```

Clamp:

```text
[0, 1]
```

### `')'`

```text
[-1, 0]
```

Clamp:

```text
[0, 0]
```

Final:

```text
minOpen == 0
```

Therefore:

```text
true
```

One valid interpretation is:

```text
(*))
 ↓
(())
```

---

# Example of Impossible Prefix

```text
s = ")*"
```

First character:

```text
')'
```

gives:

```text
minOpen = -1
maxOpen = -1
```

Since:

```cpp
maxOpen < 0
```

immediately:

```text
false
```

The `*` appearing later cannot fix an already-invalid prefix.

---

# Algorithm

```text
minOpen = 0
maxOpen = 0

For every character:

    '(':
        minOpen++
        maxOpen++

    ')':
        minOpen--
        maxOpen--

    '*':
        minOpen--
        maxOpen++

    if maxOpen < 0:
        return false

    minOpen = max(minOpen, 0)

return minOpen == 0
```

---

# Code

```cpp
class Solution {
public:
    bool checkValidString(string s) {
        int minOpen = 0;
        int maxOpen = 0;

        for(auto &c : s){
            if(c == '('){
                minOpen++;
                maxOpen++;
            }
            else if(c == ')'){
                minOpen--;
                maxOpen--;
            }
            else{
                minOpen--;
                maxOpen++;
            }

            if(maxOpen < 0)
                return false;

            minOpen = max(minOpen, 0);
        }

        return minOpen == 0;
    }
};
```

---

# Why the Initial Approach Fails

A simple approach might count:

```text
number of '('
number of ')'
number of '*'
```

and check whether `*` can compensate for the final imbalance.

The problem is that **position matters**.

For example:

```text
")("
```

has:

```text
'(' = 1
')' = 1
```

so the final balance is zero.

But it is invalid because the balance becomes negative at the first character.

Therefore, we must consider **every prefix**, not just the final counts.

---

# Complexity

For `n = s.size()`:

```text
Time  : O(n)
Space : O(1)
```

We process each character exactly once.

---

# Common Mistakes

### 1. Checking only final balance

```text
count('(') - count(')')
```

is not sufficient.

The balance must never become negative for a valid interpretation.

---

### 2. Treating every `*` the same way

Different `*` characters may need different interpretations.

One can become:

```text
(
```

while another becomes:

```text
)
```

and another can become empty.

---

### 3. Returning false whenever `minOpen < 0`

Wrong.

For example:

```text
"*"
```

can have a negative minimum interpretation, but it is still valid.

Only:

```text
maxOpen < 0
```

means **all possible interpretations are invalid**.

---

# Pattern Recognition

When you see:

```text
Parentheses

+

Wildcard character with multiple possible meanings

+

Need to maintain validity for every prefix
```

think:

```text
Range of possible balances
```

Maintain:

```text
[min possible balance,
 max possible balance]
```

rather than trying to choose the wildcard's meaning immediately.

---

# Greedy Insight

The important idea is:

> **Don't greedily decide what `*` means. Greedily maintain the entire feasible range of balances.**

```text
minOpen
   ↓
most closing interpretation

maxOpen
   ↓
most opening interpretation
```

If the upper bound becomes negative, **no solution is possible**.

If the lower bound reaches zero at the end, **at least one valid interpretation exists**.

---

# Core Insight

For `Valid Parenthesis String`, the challenge is not simply balancing the total number of `(` and `)`. **Every prefix must remain potentially valid.** Because `*` can represent `(`, `)` or nothing, we maintain the range `[minOpen, maxOpen]` of balances that are still achievable. `maxOpen < 0` means no interpretation can save the current prefix, while `minOpen == 0` at the end means at least one interpretation produces a valid parenthesis string.
