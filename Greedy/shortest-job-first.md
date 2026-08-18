# Shortest Job First (SJF) — GFG

## Reference

**Question:** [https://www.geeksforgeeks.org/problems/shortest-job-first/1](https://www.geeksforgeeks.org/problems/shortest-job-first/1)

**Pattern:**
`Greedy + Sorting`

---

# Problem

Given the burst time of `n` processes:

```text id="j0tq5h"
bt[i]
```

find the **minimum average waiting time** when the processes are executed using the **Shortest Job First (SJF)** strategy.

---

# Key Observation

To minimize the total waiting time:

> **Execute the shortest process first.**

Why?

A process that executes earlier contributes its burst time to the waiting time of **every process after it**.

Therefore, putting a long process before a short process unnecessarily increases the short process's waiting time.

So:

```text id="m4x7q2"
Sort burst times
       ↓
Smallest burst time first
       ↓
Calculate waiting times
```

---

# Example

Suppose:

```text id="9k2vpd"
bt = [3, 1, 2]
```

Sort:

```text id="z7n3qm"
[1, 2, 3]
```

### Process 1

```text id="3q8wpa"
waiting time = 0
```

### Process 2

```text id="k5v1rs"
waiting time = 1
```

because process 1 took `1` unit.

### Process 3

```text id="p2x6nc"
waiting time = 1 + 2 = 3
```

Therefore:

```text id="4m8qsy"
Total waiting time = 0 + 1 + 3 = 4
```

Average:

```text id="w3n7ka"
4 / 3
```

---

# Why Shortest First Is Optimal

Consider two processes:

```text id="v7p2mx"
A = 5
B = 2
```

### A → B

```text id="f8k3qw"
Waiting:
A = 0
B = 5

Total = 5
```

### B → A

```text id="c2m9zr"
Waiting:
B = 0
A = 2

Total = 2
```

Therefore:

```text id="d5x7vn"
shorter process first
        ↓
smaller total waiting time
```

This idea extends to the entire set of processes.

---

# Calculating Waiting Time

After sorting:

```text id="r7n3kx"
[1, 2, 3, 4]
```

waiting times are:

```text id="z8m2qp"
0
1
1 + 2 = 3
1 + 2 + 3 = 6
```

In code, maintain:

```cpp id="h4v7ns"
int start = 0;
```

Before executing each process:

```cpp id="p6q1zw"
totalWait += start;
```

Then:

```cpp id="m8k3xr"
start += bt[i];
```

So `start` represents the total burst time of all previously executed processes.

---

# Code

```cpp id="2v8nqm"
class Solution {
public:
    int solve(vector<int>& bt) {
        double totalWait = 0;
        int start = 0;

        sort(bt.begin(), bt.end());

        for(int i = 0; i < bt.size(); i++){
            totalWait += start;
            start += bt[i];
        }

        return (int)(totalWait / bt.size());
    }
};
```

---

# Complexity

Sorting:

```text id="x5q9km"
O(n log n)
```

Traversal:

```text id="k2v6rp"
O(n)
```

Overall:

```text id="c8m4yw"
Time  : O(n log n)
Space : O(1) extra
```

assuming the input array can be sorted in-place.

---

# Common Mistakes

### 1. Sorting in decreasing order

Wrong:

```text id="d4p8xm"
Longest → Shortest
```

This increases waiting time.

Correct:

```text id="m7q2vz"
Shortest → Longest
```

---

### 2. Including the current process in its own waiting time

The first process always has:

```text id="f2k9nw"
waiting time = 0
```

That's why you add `start` **before**:

```cpp id="3q6wkp"
start += bt[i];
```

---

### 3. Confusing waiting time with completion time

For:

```text id="w1m7xr"
[1, 2, 3]
```

completion times are:

```text id="9k4qvb"
1, 3, 6
```

but waiting times are:

```text id="p8x2nm"
0, 1, 3
```

The problem asks for **waiting time**, not completion time.

---

# Pattern Recognition

When the problem asks:

```text id="v5k8qy"
Minimize average waiting time
+
All processes are available at the same time
+
Non-preemptive execution
```

think:

```text id="z3m7px"
Shortest Job First
        ↓
Sort by burst time
        ↓
Execute shortest first
```

---

# Core Insight

> **A long process delays every process behind it, while a short process delays fewer units of time. Therefore, executing jobs in increasing order of burst time minimizes total and average waiting time.**

The greedy pattern is simply:

```text id="q7n2mv"
Sort by burst time ↑
        ↓
Shortest process first
        ↓
Minimum total waiting time
        ↓
Minimum average waiting time
```
