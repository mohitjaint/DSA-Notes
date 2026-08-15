# Jump Game — LeetCode 55

## Reference

**Question:** [https://leetcode.com/problems/jump-game/](https://leetcode.com/problems/jump-game/)

**Pattern:**
`Greedy + Range Expansion`

**Related Problems:**

- Jump Game II
- Gas Station
- Maximum Subarray (Kadane's Algorithm)
- Minimum Platforms

---

# Problem

Given an array:

```text id="9p8f3k"
nums[i]
```

where `nums[i]` represents the maximum jump length from index `i`.

Determine whether it is possible to reach the last index.

---

# Key Observation

At any point, we don't care about all possible paths.

We only need to know:

> **What is the farthest index we can reach so far?**

Maintain:

```cpp id="r5t8bx"
maxReach
```

which represents:

```text id="0zqv3k"
maximum reachable index from all positions visited so far
```

---

# Greedy Idea

For every index `i`:

If:

```cpp id="h9v4qk"
i <= maxReach
```

then this index is reachable.

From here, update:

```cpp id="f4x2qz"
maxReach = max(maxReach, i + nums[i])
```

because this position may extend our reachable range.

---

# Algorithm

```text id="4n7jbx"
Initialize:

maxReach = 0

For every index i:

    If i is reachable:

        extend maxReach

    Else:

        stop (cannot move further)


Return whether maxReach reaches last index
```

---

# Example 1

Input:

```text id="0x4b7h"
nums = [2,3,1,1,4]
```

Process:

### Index 0

```text id="f2t8qz"
maxReach = max(0, 0+2)
        = 2
```

Reachable:

```text id="5w0x9j"
0 → 1 → 2
```

---

### Index 1

```text id="q3v9yp"
maxReach = max(2, 1+3)
        = 4
```

Now:

```text id="r9m4q1"
last index = 4
```

is reachable.

Answer:

```text id="1m6d8p"
true
```

---

# Example 2

Input:

```text id="6v4r8z"
nums = [3,2,1,0,4]
```

Process:

```text id="u8z1px"
Index 0:

maxReach = 3
```

Indices 1,2,3 are reachable.

At index 3:

```text id="q1r5ny"
3 + nums[3]
=
3 + 0
=
3
```

Range cannot extend.

Now:

```text id="p4z6wy"
index 4 > maxReach
```

Index 4 cannot be reached.

Answer:

```text id="2k9qvw"
false
```

---

# Why the Greedy Choice Works

Suppose multiple positions are reachable:

```text id="3w7hpn"
0, 1, 2
```

Instead of exploring each jump choice, we only keep the farthest possible reach.

Example:

```text id="n8r4cz"
Position 1 reaches 5

Position 2 reaches 4
```

Position 1 dominates position 2.

There is no benefit in keeping smaller ranges.

Therefore:

> The maximum reachable index contains all useful information.

---

# Code

```cpp id="j3w8nm"
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int maxReach = 0;

        for(int i = 0; i < nums.size(); i++){
            if(i <= maxReach){
                maxReach = max(maxReach, i + nums[i]);
            }
            else{
                break;
            }
        }

        return maxReach >= nums.size() - 1;
    }
};
```

---

# Complexity

For `n = nums.size()`:

## Time

Each index is visited once.

```text id="5c8v9m"
O(n)
```

## Space

Only one variable is maintained.

```text id="2m7qkx"
O(1)
```

---

# Common Mistakes

## 1. Trying all jumps

Wrong approach:

```text id="d6h2mw"
From every index:
    try every possible jump
```

This becomes exponential.

Greedy avoids unnecessary exploration.

---

## 2. Tracking current jump only

Wrong idea:

```text id="y8k2pz"
Only consider the next jump
```

A smaller jump now may lead to a larger future reach.

Always track:

```text id="x7m3qy"
global maximum reach
```

---

## 3. Forgetting unreachable indices

Example:

```text id="a3z9qk"
[2,0,0,1]
```

At some point:

```text id="v4x6mn"
i > maxReach
```

No future index can help.

---

# Pattern Recognition

When you see:

```text id="7h4kq9"
Array of jumps

+

Need to know if destination is reachable

+

Choices at each index
```

think:

```text id="x2m7pv"
Greedy

↓

Maintain farthest reachable position
```

---

# Interview Trigger

```text id="c8m1vz"
Reachability problem

        ↓

Don't simulate jumps

        ↓

Maintain maximum reachable index

        ↓

Expand range while scanning

        ↓

If current index exceeds range:
    impossible

        ↓

If maxReach reaches end:
    possible
```

---

# Core Insight

The problem is not about finding the exact sequence of jumps. The only information that matters is the **farthest position reachable so far**.

By continuously extending this reachable boundary, we avoid exploring individual paths and solve the problem in a single pass.

```text id="m9v3xq"
Current reachable range
          ↓
Check every position inside it
          ↓
Expand range as much as possible
          ↓
Reach destination or fail
```

This is the fundamental greedy pattern behind **Jump Game**.
