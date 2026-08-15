# Jump Game II — LeetCode 45

## Reference

**Question:** [https://leetcode.com/problems/jump-game-ii/](https://leetcode.com/problems/jump-game-ii/)

**Pattern:**
`Greedy + Range Expansion`

**Related Problem:**
Jump Game — LeetCode 55

---

# Problem

Given:

```text
nums[i]
```

where `nums[i]` represents the maximum jump length from index `i`.

Return the **minimum number of jumps** required to reach the last index.

---

# Key Observation

Instead of deciding:

> "Which exact index should I jump to?"

consider the **entire range of indices reachable using the current number of jumps**.

For that range, calculate:

> **What is the farthest index any of these positions can reach?**

That farthest position becomes the boundary for the next jump.

---

# Range Representation

Your variables:

```cpp
int l = 0, r = 0;
```

represent:

```text
[l, r]
```

= all positions that can be reached using the current number of jumps.

Initially:

```text
[0, 0]
```

because before making any jump, only index `0` is reachable.

---

# Greedy Process

For every current range:

```cpp
for(int i = l; i <= r; i++){
    farthest = max(farthest, i + nums[i]);
}
```

Find the farthest position reachable from **any index in the current range**.

Then:

```cpp
jumps++;
```

because reaching that new range requires one additional jump.

And:

```cpp
l = r + 1;
r = farthest;
```

moves to the next range.

---

# Example

```text
nums = [2,3,1,1,4]
```

### Initially

```text
l = 0
r = 0
jumps = 0
```

Current range:

```text
[0]
```

From index `0`:

```text
0 + nums[0]
= 0 + 2
= 2
```

So:

```text
farthest = 2
```

Make one jump:

```text
jumps = 1

new range = [1, 2]
```

---

### Second range

Now:

```text
l = 1
r = 2
```

Check both positions:

```text
i = 1:
1 + nums[1] = 1 + 3 = 4

i = 2:
2 + nums[2] = 2 + 1 = 3
```

Therefore:

```text
farthest = 4
```

Make another jump:

```text
jumps = 2
```

Now:

```text
r = 4
```

which is the last index.

Answer:

```text
2
```

---

# Mental Model

Think of the algorithm as expanding **layers**:

```text
Jump 0:
[0]

        ↓

Jump 1:
[1 ... 2]

        ↓

Jump 2:
[3 ... 4]
```

You don't care which particular index inside a layer you jump from.

You only care about:

```text
How far can this entire layer take me?
```

---

# Algorithm

```text
l = 0
r = 0
jumps = 0

while r < last index:

    farthest = 0

    for every i in [l, r]:
        farthest = max(farthest, i + nums[i])

    jumps++

    l = r + 1
    r = farthest

return jumps
```

---

# Code

```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        int n = nums.size();

        int l = 0, r = 0, jumps = 0;

        while(r < n - 1) {
            int farthest = 0;

            for(int i = l; i <= r; i++) {
                farthest = max(farthest, i + nums[i]);
            }

            jumps++;
            l = r + 1;
            r = farthest;
        }

        return jumps;
    }
};
```

---

# Why This Is Greedy

Suppose the current jump allows us to reach:

```text
[1, 4]
```

We could choose any position in this range as the next jumping position.

Instead of committing to one, we calculate:

```text
max(i + nums[i])
```

over the entire range.

Choosing the **farthest possible next boundary** cannot hurt because it gives us at least as many future options as any smaller boundary.

Therefore:

> **At every jump, maximize the reachable boundary.**

---

# Why We Don't Need to Track Individual Paths

Imagine:

```text
Current reachable range:

[1, 4]
```

Maybe:

```text
1 → 5
2 → 6
3 → 4
4 → 7
```

We don't need to remember all these possibilities.

The only useful information is:

```text
farthest = 7
```

because every position up to the current boundary is already reachable.

---

# Complexity

Although there is a nested loop, the overall complexity is:

```text
O(n)
```

because each index belongs to exactly one `[l, r]` range.

Therefore:

```text
Time  : O(n)
Space : O(1)
```

This is the optimal solution.

---

# Common Mistakes

### 1. Choosing a specific next index

You don't need to decide:

```text
"Jump from index 1"
```

Instead:

```text
Find the farthest reach among all currently reachable indices.
```

---

### 2. Confusing `r` with the global farthest

`r` represents:

```text
end of the current jump's reachable range
```

while:

```cpp
farthest
```

represents:

```text
end of the next reachable range
```

---

### 3. Updating the range while scanning it

Don't modify `l` or `r` inside:

```cpp
for(int i = l; i <= r; i++)
```

First calculate the entire `farthest`, then update:

```cpp
l = r + 1;
r = farthest;
```

---

# Jump Game I vs Jump Game II

|                 | Jump Game I             | Jump Game II                     |
| --------------- | ----------------------- | -------------------------------- |
| Goal            | Can we reach end?       | Minimum jumps                    |
| Greedy variable | `maxReach`              | `[l, r]` + `farthest`            |
| Main idea       | Expand reachable region | Expand reachable region per jump |
| Time            | O(n)                    | O(n)                             |
| Space           | O(1)                    | O(1)                             |

### Jump Game I

```text
"Can I reach the end?"
        ↓
Track farthest reachable index
```

### Jump Game II

```text
"How many jumps do I need?"
        ↓
Track the current reachable range
        ↓
Find the farthest next range
        ↓
Count range expansions
```

---

# Pattern Recognition

When you see:

```text
Minimum number of jumps
+
Each position has a maximum reach
```

think:

```text
Greedy
    ↓
Current reachable range
    ↓
Find farthest next reach
    ↓
Expand range
    ↓
Count jumps
```

---

# Interview Trigger

```text
Jump Game II

        ↓

Need minimum jumps

        ↓

Don't choose an exact path

        ↓

Maintain [l, r]

        ↓

Scan the entire current range

        ↓

Find farthest reachable position

        ↓

One more jump

        ↓

Move to next range

        ↓

Repeat until destination is inside range
```

---

# Core Insight

> **Treat every set of positions reachable with the same number of jumps as one range.**

For the current range `[l, r]`, find the maximum:

```text
i + nums[i]
```

over all `i` in that range. That maximum becomes the boundary reachable with one additional jump.

So the core pattern is:

```text
Current range
      ↓
Maximum reach from this range
      ↓
Next range
      ↓
+1 jump
```

This gives the optimal **O(n) time, O(1) space** greedy solution.
