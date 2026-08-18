# Candy — LeetCode 135

## Reference

**Question:** [https://leetcode.com/problems/candy/](https://leetcode.com/problems/candy/)

**Pattern:**
`Greedy + Two Passes`

**Alternative:**
`Greedy + Slope / Peak-Valley — O(1) Space`

---

# Problem

There are `n` children standing in a line.

Each child has a rating.

We must give candies such that:

1. Every child gets **at least 1 candy**.
2. If a child has a higher rating than an adjacent child, they must get **more candies**.

Return the **minimum total number of candies**.

---

# Key Observation

Each child has **two possible constraints**:

### Left neighbor

If:

```text id="6c9a7x"
ratings[i] > ratings[i-1]
```

then:

```text id="m4u1c9"
candies[i] > candies[i-1]
```

### Right neighbor

If:

```text id="q5r8wa"
ratings[i] > ratings[i+1]
```

then:

```text id="n7k3px"
candies[i] > candies[i+1]
```

The important insight is:

> **Handle the left and right constraints separately, then combine them.**

---

# Approach 1 — Two Passes

Start every child with:

```cpp id="b8q0fz"
allocations[i] = 1;
```

Then make two passes.

---

## Pass 1 — Left to Right

Handle the constraint involving the **left neighbor**.

```cpp id="2o5x1m"
for(int i = 1; i < n; i++){
    if(ratings[i] > ratings[i-1]){
        allocations[i] = allocations[i-1] + 1;
    }
}
```

Example:

```text id="j2l8mc"
ratings:
1  2  3  4

candies:
1  2  3  4
```

Every time the rating increases, the candy count increases by one.

---

# Pass 2 — Right to Left

Now handle the constraint involving the **right neighbor**.

```cpp id="9c4q7z"
for(int i = n-2; i >= 0; i--){
    if(ratings[i] > ratings[i+1]){
        allocations[i] =
            max(allocations[i], allocations[i+1] + 1);
    }
}
```

### Why `max()`?

Because the child may already have a candy requirement from the left-to-right pass.

We must satisfy **both** constraints.

Example:

```text id="v8q2am"
ratings:
1 2 3 2 1
```

After left → right:

```text id="x3g6pn"
candies:
1 2 3 1 1
```

Now right → left:

```text id="k2z8qs"
1 2 3 2 1
```

The middle child already has `3`, so we don't overwrite it with a smaller value.

Hence:

```cpp id="u7s4mk"
max(current, requiredFromRight)
```

---

# Complete Approach 1

```cpp id="6t7n2x"
class Solution {
public:
    int candy(vector<int>& ratings) {
        int n = ratings.size();

        vector<int> allocations(n, 1);

        // Left to right
        for(int i = 1; i < n; i++){
            if(ratings[i] > ratings[i-1]){
                allocations[i] = allocations[i-1] + 1;
            }
        }

        // Right to left
        for(int i = n-2; i >= 0; i--){
            if(ratings[i] > ratings[i+1]){
                allocations[i] =
                    max(allocations[i],
                        allocations[i+1] + 1);
            }
        }

        return accumulate(
            allocations.begin(),
            allocations.end(),
            0
        );
    }
};
```

---

# Example

Consider:

```text id="4n8w2c"
ratings = [1, 0, 2]
```

Initially:

```text id="f7c3mk"
candies = [1, 1, 1]
```

### Left → Right

At index 1:

```text id="g7p1ks"
0 < 1
```

No change.

At index 2:

```text id="m4r9za"
2 > 0
```

So:

```text id="a5k3px"
candies = [1, 1, 2]
```

### Right → Left

At index 1:

```text id="h1c6yn"
0 < 2
```

No change.

At index 0:

```text id="w8j4qv"
1 > 0
```

Therefore:

```text id="d3x9km"
candies[0] = candies[1] + 1
           = 2
```

Final:

```text id="0v5qsz"
[2, 1, 2]
```

Total:

```text id="k8n4yd"
5
```

---

# Why One Pass Is Not Enough

Consider:

```text id="y3q7vp"
ratings = [1, 2, 3, 2, 1]
```

The center child needs to satisfy both sides:

```text id="q5n2ka"
1 2 3 2 1
    ↑
```

It needs more candies than both neighbors.

A single left-to-right pass handles:

```text id="a2m7kx"
1 2 3
```

but cannot know the complete requirement from the right side yet.

Similarly, a right-to-left pass alone cannot handle everything.

Therefore, two directional passes are a natural solution.

---

# Approach 2 — Peak / Valley

Your second solution removes the `allocations` array.

Instead of explicitly assigning candies, it counts the required candies based on the **shape of the rating sequence**.

There are three possible patterns:

### Increasing

```text id="2w7p3n"
1 2 3 4
```

Candy requirement:

```text id="c9v5km"
1 2 3 4
```

---

### Decreasing

```text id="8x4q1p"
4 3 2 1
```

Candy requirement:

```text id="g5k2zs"
4 3 2 1
```

---

### Peak

```text id="7m3n8c"
1 2 3 4 3 2 1
        ↑
       peak
```

The peak belongs to **both** the increasing and decreasing sequences.

If:

```text id="4z8k1p"
peak length < decreasing length
```

the peak doesn't have enough candies to satisfy the decreasing side.

Your correction:

```cpp id="h7p4zc"
if(down > peak){
    sum += down - peak;
}
```

adds the required extra candies to the peak.

---

# Approach 2 Complexity

```text id="q9m3vx"
Time  : O(n)
Space : O(1)
```

This is more space-efficient than Approach 1.

---

# Comparison

| Approach        | Idea                                          | Time |    Space |
| --------------- | --------------------------------------------- | ---: | -------: |
| **Two Passes**  | Build candy requirements from both directions | O(n) |     O(n) |
| **Peak/Valley** | Count increasing/decreasing slopes            | O(n) | **O(1)** |

### Recommended

For learning/interviews:

**Approach 1 → primary solution**

It is easier to reason about and remember.

**Approach 2 → optimization**

Know it when the interviewer asks:

> "Can you do it in O(1) extra space?"

---

# Common Mistakes

### 1. Only doing left → right

This fails on decreasing sequences.

Example:

```text id="j3n7qw"
3 2 1
```

---

### 2. Only doing right → left

This fails on increasing sequences.

```text id="r8m2xc"
1 2 3
```

---

### 3. Overwriting instead of taking `max`

Wrong:

```cpp id="n2w8qm"
allocations[i] = allocations[i+1] + 1;
```

Correct:

```cpp id="h4q6zs"
allocations[i] =
    max(allocations[i],
        allocations[i+1] + 1);
```

A child may need to satisfy constraints from **both directions**.

---

### 4. Assuming the highest-rated child always gets the most candies

Not necessarily.

Only **adjacent comparisons** matter.

For:

```text id="p5k8yr"
ratings = [1, 3, 2]
```

the allocation can be:

```text id="x7m2cq"
[1, 2, 1]
```

---

# Pattern Recognition

When you see:

```text id="z6p3wk"
Array

+

Adjacent relationship

+

Need to satisfy constraints from both left and right
```

consider:

```text id="m8q2yv"
Two-direction greedy
        ↓
Left → Right
        ↓
Right → Left
        ↓
Combine using max
```

This is the main pattern to remember from this problem.

---

# Core Insight

> **The candy requirement for each child is the maximum requirement imposed by its left and right neighbors.**

So:

```text id="f2n7ka"
Left constraint
      ↓
Left → Right pass
      +
Right constraint
      ↓
Right → Left pass
      ↓
Take maximum
      ↓
Minimum valid allocation
```

And the optimized version recognizes the same constraints through **increasing and decreasing slopes**, allowing the extra array to be removed.
