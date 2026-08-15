# Job Sequencing with Deadlines — GFG

## Reference

**Question:** [https://www.geeksforgeeks.org/problems/job-sequencing-problem-1587115620/1](https://www.geeksforgeeks.org/problems/job-sequencing-problem-1587115620/1)

**Pattern:**
`Greedy + Sorting + DSU (Disjoint Set Union)`

---

# Problem

Each job has:

```text
deadline → latest time slot by which it must be completed
profit   → profit obtained if the job is completed
```

Every job takes **exactly 1 unit of time**.

We need to:

1. Maximize the **number of jobs** completed.
2. Maximize the **total profit**.

The standard greedy objective is to maximize total profit; the returned answer contains:

```text
[number of jobs, total profit]
```

---

# Key Greedy Insight

Process jobs in **decreasing order of profit**.

```text
Highest profit
      ↓
Next highest profit
      ↓
...
```

For every job:

> Put it in the **latest available slot ≤ its deadline**.

So there are two greedy decisions:

```text
1. Which job?
   → Highest profit first

2. Which slot?
   → Latest available slot before its deadline
```

---

# Why Highest Profit First?

Suppose:

```text
Job A → profit 100
Job B → profit 50
```

If both compete for the same available time slots, we should try to schedule the job giving the larger profit first.

Thus:

```cpp
sort(v.begin(), v.end(), comp);
```

where:

```cpp
return a.first > b.first;
```

because:

```text
v[i].first = profit
v[i].second = deadline
```

---

# Why Latest Available Slot?

Suppose a job has:

```text
deadline = 4
```

and slots:

```text
1  2  3  4
```

If all are free, put the job at:

```text
slot 4
```

rather than slot 1.

Why?

Because earlier slots may be needed by jobs with smaller deadlines.

Therefore:

> **Schedule each selected job as late as possible.**

This preserves the maximum number of earlier slots for other jobs.

---

# Basic Approach

Without DSU, we can search backwards:

```text
deadline
   ↓
deadline - 1
   ↓
deadline - 2
   ↓
...
```

until we find a free slot.

For example:

```cpp
for(int j = deadline; j > 0; j--){
    if(timeSlot[j] == 0){
        // schedule job
        break;
    }
}
```

This is conceptually simple, but can take:

```text
O(n × maxDeadline)
```

and may TLE.

---

# DSU Optimization

The expensive operation is:

> **Find the latest free slot ≤ deadline.**

DSU lets us perform this efficiently.

Maintain:

```cpp
vector<int> parent;
```

with the meaning:

```text
parent[x] = latest available slot at or before x
```

Initially:

```text
parent[i] = i
```

because every slot is available.

---

# `find()`

```cpp
int find(vector<int>& parent, int x) {
    if(parent[x] == x)
        return x;

    return parent[x] = find(parent, parent[x]);
}
```

This uses **path compression**.

If a slot is unavailable, `find()` jumps toward the previous available slot instead of repeatedly checking every occupied position.

---

# Example of DSU

Suppose slots are:

```text
1  2  3  4
```

Initially:

```text
parent[1] = 1
parent[2] = 2
parent[3] = 3
parent[4] = 4
```

So:

```text
find(4) = 4
```

means slot 4 is available.

After using slot 4:

```cpp
parent[4] = find(parent, 3);
```

Now:

```text
find(4) → 3
```

So the next job requesting a slot ≤ 4 gets slot 3.

If slot 3 is then occupied:

```cpp
parent[3] = find(parent, 2);
```

Now:

```text
find(4) → 2
```

Path compression makes these chains very efficient.

---

# Slot `0`

We use slot `0` as a sentinel:

```text
slot 0 = no valid slot available
```

Therefore:

```cpp
if(slot > 0)
```

means a valid slot was found.

---

# Algorithm

```text
Find maximum deadline

Create DSU for slots [0 ... maxDeadline]

Sort jobs by decreasing profit

For every job:

    slot = find(deadline)

    if slot > 0:

        schedule job

        count++

        add profit

        mark slot as occupied:
            parent[slot] = find(slot - 1)

Return:
    count
    total profit
```

---

# Code

```cpp
bool comp(pair<int,int> &a, pair<int,int> &b){
    return a.first > b.first;
}

class Solution {
public:
    int find(vector<int>& parent, int x) {
        if(parent[x] == x)
            return x;

        return parent[x] = find(parent, parent[x]);
    }

    vector<int> jobSequencing(vector<int> &deadline,
                              vector<int> &profit) {

        int n = deadline.size();

        int maxDeadline =
            *max_element(deadline.begin(), deadline.end());

        vector<pair<int,int>> v;

        for(int i = 0; i < n; i++){
            v.push_back({profit[i], deadline[i]});
        }

        sort(v.begin(), v.end(), comp);

        vector<int> parent(maxDeadline + 1);

        for(int i = 0; i <= maxDeadline; i++){
            parent[i] = i;
        }

        int totalProfit = 0;
        int cnt = 0;

        for(int i = 0; i < n; i++){

            int slot = find(parent, v[i].second);

            if(slot > 0){
                cnt++;
                totalProfit += v[i].first;

                // Mark current slot occupied
                parent[slot] = find(parent, slot - 1);
            }
        }

        return {cnt, totalProfit};
    }
};
```

---

# Example

Suppose:

```text
deadline = [2, 1, 2, 1]
profit   = [100, 50, 10, 20]
```

Represent jobs as:

```text
(profit, deadline)

(100, 2)
(50, 1)
(10, 2)
(20, 1)
```

Sort:

```text
(100,2)
(50,1)
(20,1)
(10,2)
```

### Job `(100,2)`

```text
find(2) = 2
```

Schedule at slot 2.

```text
parent[2] = find(1)
```

---

### Job `(50,1)`

```text
find(1) = 1
```

Schedule at slot 1.

```text
parent[1] = find(0)
```

---

### Job `(20,1)`

```text
find(1) = 0
```

No available slot.

Skip.

---

### Job `(10,2)`

```text
find(2) = 0
```

No available slot.

Skip.

Final:

```text
count = 2
profit = 150
```

---

# Complexity

Let:

```text
n = number of jobs
D = maximum deadline
```

### Sorting

```text
O(n log n)
```

### DSU operations

Each `find()` is approximately:

```text
O(α(D))
```

where `α` is the inverse Ackermann function and is effectively constant for practical input sizes.

Therefore:

```text
Time ≈ O(n log n)
Space = O(D)
```

This is significantly better than the original slot-search approach:

```text
O(n × D)
```

which caused your TLE.

---

# Greedy + DSU: Important Distinction

DSU is **not the greedy strategy**.

The algorithm has two separate components:

```text
                Job Sequencing
                     │
          ┌──────────┴──────────┐
          ↓                     ↓
       Greedy                  DSU
          │                     │
   Which job?              Which slot?
          │                     │
   Highest profit          Latest available
      first                slot ≤ deadline
```

So the primary pattern is:

```text
Greedy
```

while DSU is the **data structure used to optimize slot allocation**.

---

# Common Mistakes

### 1. Sorting by deadline

The primary greedy criterion is **profit**, not deadline.

```text
Wrong:
sort by deadline

Correct:
sort by decreasing profit
```

---

### 2. Scheduling at the earliest available slot

Wrong:

```text
deadline = 5
use slot 1
```

Correct:

```text
deadline = 5
use latest free slot ≤ 5
```

---

### 3. Using the job index for DSU

The DSU query must use:

```cpp
find(parent, v[i].second)
```

because:

```text
v[i].second = deadline
```

Not:

```cpp
find(parent, i)
```

The job's position in the sorted array has nothing to do with its available time slots.

---

### 4. Wrong DSU size

If:

```text
maxDeadline = D
```

you need:

```cpp
vector<int> parent(D + 1);
```

because slot `D` must be accessible.

---

### 5. Forgetting to redirect an occupied slot

After scheduling at `slot`:

```cpp
parent[slot] = find(parent, slot - 1);
```

This tells DSU:

> "Slot `slot` is no longer available; the next candidate is the previous available slot."

---

# Pattern Recognition

When a problem says:

```text
Jobs

+

Each job takes one unit of time

+

Each job has a deadline

+

Each job has a profit

+

Maximize profit
```

think:

```text
Job Sequencing
       ↓
Sort by decreasing profit
       ↓
Place at latest available slot
```

If finding the latest available slot repeatedly is too slow:

```text
Add DSU
```

---

# Interview Trigger

```text
Unit-time jobs
      +
Deadlines
      +
Profit
      ↓
Sort by decreasing profit
      ↓
For each job:
    find latest free slot ≤ deadline
      ↓
If available:
    schedule
      ↓
Mark slot unavailable
      ↓
DSU can optimize slot lookup
```

---

# Core Insight

The problem combines **two ideas**:

> **Greedy:** Take the highest-profit jobs first and place each one as late as possible before its deadline.

> **DSU:** Efficiently find the latest still-available slot and redirect occupied slots to the previous available slot.

The most important mental model is:

```text
Highest profit
      ↓
Latest possible slot
      ↓
Preserve earlier slots
      ↓
DSU makes slot lookup fast
```

This turns your original `O(nD)` slot-search implementation into an approximately **O(n log n)** solution.
