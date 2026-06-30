# Boolean Parenthesization

## References

**Question:**

[https://www.geeksforgeeks.org/problems/boolean-parenthesization5610/1](https://www.geeksforgeeks.org/problems/boolean-parenthesization5610/1)

**Related Problems:**

- Matrix Chain Multiplication
- Burst Balloons
- Minimum Cost to Cut a Stick
- Boolean Expression Evaluation (Different Problem)

---

# Pattern

```text
Partition DP

Interval DP

Count DP
```

---

# Problem Statement

Given a boolean expression consisting of

```text
T
F
&
|
^
```

Count the number of ways to parenthesize the expression so that it evaluates to **True**.

---

# Key Observation

The parentheses are **not fixed**.

We are free to choose

```text
Which operator is evaluated last.
```

Every choice divides the expression into

```text
Left Expression

+

Right Expression
```

which become independent.

Hence,

```text
Partition DP
```

---

# DP State

Unlike MCM,

the answer for an interval is **not unique**.

The same interval can evaluate to

```text
True

or

False
```

Therefore,

```cpp
dp[i][j][isTrue]
```

Meaning

```text
Number of ways

substring i...j

evaluates to

True/False.
```

---

# Base Cases

### Invalid Expression

```cpp
if(i > j)
    return 0;
```

---

### Single Operand

```cpp
if(i == j)
```

If

```text
isTrue = True
```

return

```cpp
s[i]=='T'
```

Else

```cpp
s[i]=='F'
```

---

# Transition

Choose every operator as the **last operation**.

```text
T | F & T ^ F
    ^
    k
```

Partition

```text
Left

Right
```

Compute

```text
Left True

Left False

Right True

Right False
```

Combine according to the operator.

---

# Truth Tables

## '&'

### True

```text
TT
```

Ways

```cpp
LT × RT
```

---

### False

```text
TF

FT

FF
```

Ways

```cpp
LT×RF

+

LF×RT

+

LF×RF
```

---

## '|'

### True

```text
TT

TF

FT
```

Ways

```cpp
LT×RT

+

LT×RF

+

LF×RT
```

---

### False

```text
FF
```

Ways

```cpp
LF×RF
```

---

## '^'

### True

```text
TF

FT
```

Ways

```cpp
LT×RF

+

LF×RT
```

---

### False

```text
TT

FF
```

Ways

```cpp
LT×RT

+

LF×RF
```

---

# Recurrence

For every operator

```cpp
for(k=i+1;k<=j-1;k+=2)
```

Compute

```text
LT

LF

RT

RF
```

Then apply the corresponding truth table.

---

# Memoization

State

```cpp
f(i,j,isTrue)
```

Transition

```text
Choose every operator

↓

Solve Left

↓

Solve Right

↓

Merge using Truth Table

↓

Add all possible ways
```

---

# Tabulation

## DP Dependency

```cpp
dp[i][j]
```

depends on

```cpp
dp[i][k-1]

dp[k+1][j]
```

Both are **smaller intervals**.

Therefore,

compute intervals in increasing order of length.

---

# Gap / Length Iteration

Only **odd-length** substrings can form valid expressions.

Example

```text
T

T|F

T|F&T

T|F&T^T
```

Lengths

```text
1

3

5

7
```

Hence

```cpp
for(int len=3; len<=n; len+=2){

    for(int i=0; i+len-1<n; i+=2){

        int j=i+len-1;

        ...
    }
}
```

---

# Why Only Odd Length?

Expression format

```text
Operand Operator Operand Operator Operand
```

Example

```text
T|F&T
```

Indices

```text
0 1 2 3 4
```

Operands

```text
Even indices
```

Operators

```text
Odd indices
```

Therefore,

every valid expression contains

```text
Operand

Operator

Operand

...

Operand
```

which always has an **odd** number of characters.

---

# Complexity

Let

```text
n = length of expression
```

States

```text
O(n² × 2)
```

Transitions

```text
O(n)
```

Overall

```text
Time : O(n³)

Space : O(n²)
```

---

# Common Mistake #1

Using

```cpp
dp[i][j]
```

instead of

```cpp
dp[i][j][2]
```

Need both

```text
True

False
```

counts.

---

# Common Mistake #2

Forgetting to multiply counts.

Wrong

```cpp
LT + RT
```

Correct

```cpp
LT × RT
```

because every left way combines with every right way.

---

# Common Mistake #3

Incorrect Truth Table.

Always derive

```text
TT

TF

FT

FF
```

instead of memorizing formulas.

---

# Common Mistake #4

Iterating over every substring.

Only odd-length substrings represent valid expressions.

Use

```cpp
len += 2

i += 2
```

---

# Common Mistake #5

Partitioning at operands.

Partition only at

```text
Operators
```

Hence

```cpp
k += 2
```

---

# Relation with Other Partition DP Problems

| Problem                     | State      | Partition       | Merge               |
| --------------------------- | ---------- | --------------- | ------------------- |
| Matrix Chain Multiplication | Matrices   | Partition point | Multiplication Cost |
| Minimum Cost to Cut Stick   | Boundaries | First Cut       | Stick Length        |
| Burst Balloons              | Balloons   | Last Balloon    | Coins Earned        |
| Boolean Parenthesization    | Expression | Operator        | Truth Table         |

The recurrence structure remains identical.

Only the **merge logic** changes.

---

# Pattern Recognition

Whenever you see

```text
Choose one partition

↓

Split into Left + Right

↓

Compute both independently

↓

Merge according to some rule

↓

Take Min / Max / Count
```

think

```text
Partition DP
```

If the merge depends on multiple possible outcomes (like True/False),

extend the DP state.

---

# Interview Trigger

```text
Boolean Parenthesization

↓

Partition DP

↓

State

dp[i][j][isTrue]

↓

Choose every operator

↓

Compute

LT, LF, RT, RF

↓

Apply Truth Table

↓

Sum all valid ways

↓

Gap Method

↓

Only odd-length intervals

↓

O(n³)
```

---

# Core Insight

Unlike other Partition DP problems that compute a single value (minimum, maximum, or cost), **Boolean Parenthesization** requires tracking **two possible outcomes** for every interval: **True** and **False**. The partitioning pattern remains the same—choose an operator, solve the left and right subexpressions independently—but the merge step is performed using the operator's truth table. The key realization is that every valid expression has an **odd length**, so tabulation naturally proceeds by increasing odd-length intervals using the gap method.
