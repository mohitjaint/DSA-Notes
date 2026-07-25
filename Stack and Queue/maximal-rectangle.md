# Maximal Rectangle (LeetCode 85)

## References

**Question:**

[https://leetcode.com/problems/maximal-rectangle/](https://leetcode.com/problems/maximal-rectangle/)

**Related Problems:**

- Largest Rectangle in Histogram
- Maximal Square
- Trapping Rain Water
- Largest Rectangle in Histogram (One Pass Stack)

---

# Pattern

```text id="t9wq2g"
Matrix

Histogram Transformation

Monotonic Increasing Stack
```

---

# Problem Statement

Given a binary matrix,

find the area of the largest rectangle containing only

```text id="jgc3wf"
1's
```

---

# Brute Force

For every cell,

try every possible rectangle.

Check whether every cell inside it is

```text id="q5d8z6"
1
```

---

## Complexity

```text id="3n9h2m"
Time  : O(m²n²)

Space : O(1)
```

Too slow.

---

# Key Observation

A rectangle can be identified by its **bottom row**.

Suppose we process the matrix row by row.

For every column,

keep track of

```text id="z3bqgi"
How many consecutive 1's
end at the current row.
```

This converts every row into a histogram.

---

# Histogram Transformation

Maintain

```cpp id="gkn3fd"
heights[j]
```

For every row

If

```text id="axm4i3"
matrix[i][j] == '1'
```

then

```cpp id="l2m3vx"
heights[j]++;
```

Otherwise

```cpp id="6e4dy9"
heights[j] = 0;
```

---

# Example

Matrix

```text id="7hwd8u"
1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0
```

---

Row 1

```text id="5ewyyg"
Heights

1 0 1 0 0
```

Largest rectangle in histogram

↓

Area = 1

---

Row 2

```text id="2njmvw"
Heights

2 0 2 1 1
```

Solve histogram again.

---

Row 3

```text id="7wdm9v"
Heights

3 1 3 2 2
```

Largest histogram rectangle

↓

Area = 6

---

Row 4

```text id="tvnyhx"
Heights

4 0 0 3 0
```

Again solve histogram.

Maximum over all rows

↓

Answer.

---

# Algorithm

```text id="jlwm85"
Initialize heights[]

↓

For every row

↓

Update heights

↓

Treat heights as histogram

↓

Find largest rectangle in histogram

↓

Update answer

↓

Return maximum
```

---

# Largest Rectangle in Histogram

For every row,

reuse the histogram algorithm.

Maintain a **monotonic increasing stack**.

Whenever

```text id="vkmfrd"
Current height

<

Stack top height
```

pop bars.

For every popped bar

```text id="fywqna"
Current index

↓

Next Smaller Element

------------------

New stack top

↓

Previous Smaller Element
```

Area

```cpp id="f1s8kb"
height * (nse - pse - 1)
```

---

# Why Does This Work?

Suppose the largest rectangle ends at row

```text id="0snvt4"
i
```

Then

its height is exactly

```text id="pshf1r"
Number of consecutive 1's
ending at row i
```

which is precisely stored in

```cpp id="l3nkx5"
heights[]
```

Therefore,

processing every row guarantees that every possible maximal rectangle is considered once—when its **bottom edge** is reached.

---

# Example Walkthrough

Current heights

```text id="mjlwm8"
3 1 3 2 2
```

Interpret as histogram.

Largest rectangle becomes

```text id="jlwm4d"
Height = 2

Width = 3

Area = 6
```

This corresponds to

```text id="5n2gxu"
2 consecutive rows

×

3 consecutive columns
```

in the original matrix.

---

# Complexity

Let

```text id="h8wxqa"
m = rows

n = columns
```

Updating histogram

```text id="jlwm81"
O(n)
```

Largest Rectangle

```text id="jlwm82"
O(n)
```

Performed for every row.

Overall

```text id="jlwm83"
Time  : O(mn)

Space : O(n)
```

---

# Common Mistake #1

Resetting heights incorrectly.

Correct

```cpp id="jlwm84"
if(matrix[i][j]=='1')
    heights[j]++;
else
    heights[j]=0;
```

Do **not** decrement heights.

---

# Common Mistake #2

Rebuilding the histogram from scratch for every row.

Only update the previous histogram.

---

# Common Mistake #3

Forgetting that

```text id="jlwm86"
heights[j]
```

represents **consecutive** ones, not the total count of ones in a column.

---

# Common Mistake #4

Using the histogram algorithm incorrectly.

Remember,

the stack stores **indices**, not heights.

Correct comparison

```cpp id="jlwm87"
heights[st.top()] >= heights[i]
```

---

# Common Mistake #5

Ignoring an empty matrix.

Handle

```cpp id="jlwm88"
if(matrix.empty() || matrix[0].empty())
    return 0;
```

before accessing

```cpp id="jlwm89"
matrix[0]
```

---

# Comparison

| Problem                        | Transformation     | Final Algorithm             |
| ------------------------------ | ------------------ | --------------------------- |
| Largest Rectangle in Histogram | None               | Monotonic Stack             |
| Maximal Rectangle              | Matrix → Histogram | Histogram + Monotonic Stack |
| Maximal Square                 | Matrix → DP        | Dynamic Programming         |

---

# Pattern Recognition

Whenever you see

```text id="jlwm90"
Binary matrix

↓

Need largest rectangle

↓

Rectangles grow vertically
```

think

```text id="jlwm91"
Convert each row

↓

Histogram

↓

Solve Largest Rectangle in Histogram
```

Many 2D problems become much easier after transforming them into a familiar 1D problem.

---

# Interview Trigger

```text id="jlwm92"
Binary matrix

↓

Maintain consecutive heights

↓

Each row becomes a histogram

↓

Reuse Largest Rectangle in Histogram

↓

Monotonic Increasing Stack

↓

Current index = NSE

↓

Stack top = PSE

↓

Area = Height × Width

↓

Maximum over all rows

↓

O(mn)
```

---

# Core Insight

The key idea is to **reduce a 2D problem into a sequence of 1D problems**. For each row, maintain the height of consecutive `1`s in every column, effectively transforming the row into a histogram. The largest rectangle ending at that row is exactly the **Largest Rectangle in Histogram** problem. By solving this histogram for every row and taking the maximum area, we obtain the largest rectangle in the entire matrix in **O(mn)** time.
