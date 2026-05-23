# Course Schedule II — Kahn’s Algorithm (Topological Sort)

Problem : [LeetCode - Course Schedule II](https://leetcode.com/problems/course-schedule-ii/description/?utm_source=chatgpt.com)

---

# Problem

Given:

```txt id="jlwm1"
numCourses
prerequisites
```

Each pair:

```txt id="jlwm2"
[a,b]
```

means:

```txt id="wjglwm3"
take b
before a
```

Need:

```txt id="wjglwm4"
return valid course order
```

If impossible:

```txt id="wjglwm5"
return {}
```

---

Example:

Input:

```txt id="wjglwm6"
numCourses=4

[
[1,0],
[2,0],
[3,1],
[3,2]
]
```

Graph:

```txt id="wjglwm7"
0 → 1
↓

2

↓

3
```

Output:

```txt id="wjglwm8"
[0,1,2,3]
```

or

```txt id="wjglwm9"
[0,2,1,3]
```

---

# Graph Interpretation

Think:

```txt id="wjglwm10"
course = node
```

Edge:

```txt id="wjglwm11"
prerequisite
→
dependent course
```

This becomes:

```txt id="wjglwm12"
Topological Sorting
```

---

# Key Observation

A course can be taken only if:

```txt id="wjglwm13"
all prerequisites completed
```

Instead of repeatedly checking:

```txt id="wjglwm14"
are prerequisites done?
```

Store:

```txt id="wjglwm15"
remaining prerequisites
```

This is called:

```txt id="wjglwm16"
indegree
```

---

# What is Indegree?

For every node:

```txt id="wjglwm17"
indegree[node]
=
incoming edges
```

Meaning:

```txt id="wjglwm18"
number of prerequisites left
```

Example:

```txt id="wjglwm19"
0 → 1
0 → 2
1 → 3
2 → 3
```

Indegree:

```txt id="wjglwm20"
0 : 0

1 : 1

2 : 1

3 : 2
```

---

# Core Idea (Kahn's Algorithm)

### Step 1

Build:

```cpp id="wjglwm21"
adj
```

and

```cpp id="wjglwm22"
indegree
```

---

### Step 2

Push all:

```txt id="wjglwm23"
indegree==0
```

into queue.

These courses require nothing.

---

### Step 3

Take course.

Add to answer.

---

### Step 4

Remove its effect.

For every child:

```cpp id="wjglwm24"
indegree[child]--
```

If:

```cpp id="wjglwm25"
indegree==0
```

push.

---

### Step 5

If:

```txt id="wjglwm26"
answer size
!=
numCourses
```

Cycle exists.

Return:

```txt id="wjglwm27"
{}
```

---

# Visual Example

Input:

```txt id="wjglwm28"
0 → 1
0 → 2
1 → 3
2 → 3
```

Initial:

```txt id="wjglwm29"
indegree

0:0
1:1
2:1
3:2
```

Queue:

```txt id="wjglwm30"
[0]
```

Take:

```txt id="wjglwm31"
0
```

Update:

```txt id="wjglwm32"
1→0

2→0
```

Queue:

```txt id="wjglwm33"
[1,2]
```

Take:

```txt id="wjglwm34"
1
```

Update:

```txt id="wjglwm35"
3→1
```

Take:

```txt id="wjglwm36"
2
```

Update:

```txt id="wjglwm37"
3→0
```

Queue:

```txt id="wjglwm38"
[3]
```

Answer:

```txt id="wjglwm39"
[0,1,2,3]
```

---

# Final Solution

```cpp id="wjglwm40"
class Solution {
public:

    vector<int> findOrder(
        int numCourses,
        vector<vector<int>>& prerequisites
    ) {

        vector<int>
            adj[numCourses];

        vector<int>
            indegree(
                numCourses,
                0
            );

        for (
            auto& p :
            prerequisites
        ) {

            adj[p[1]]
                .push_back(
                    p[0]
                );

            indegree[
                p[0]
            ]++;
        }

        queue<int> q;

        for (
            int i=0;
            i<numCourses;
            i++
        ) {

            if (
                indegree[i]
                ==
                0
            ) {

                q.push(i);
            }
        }

        vector<int> ans;

        while (
            !q.empty()
        ) {

            int x =
                q.front();

            q.pop();

            ans.push_back(x);

            for (
                auto& child :
                adj[x]
            ) {

                indegree[
                    child
                ]--;

                if (
                    indegree[
                        child
                    ]
                    ==
                    0
                ) {

                    q.push(
                        child
                    );
                }
            }
        }

        if (
            ans.size()
            !=
            numCourses
        )
            return {};

        return ans;
    }
};
```

---

# Complexity

Let:

- `V` = courses
- `E` = prerequisites

Time:

O(V+E)

Reason:

- every node pushed once
- every edge processed once

---

Space:

O(V+E)

---

# Why This Is Better Than Manual Checking

Old idea:

```txt id="wjglwm41"
for every child

scan all prerequisites
```

Time:

O(VE)

---

Kahn:

```txt id="wjglwm42"
store remaining prerequisites
```

Update:

```txt id="wjglwm43"
O(1)
```

---

# Common Mistakes

❌ Forgetting:

```cpp id="wjglwm44"
indegree[child]--
```

---

❌ Pushing duplicate nodes

---

❌ Returning order without checking size

---

❌ Wrong edge direction

Correct:

```txt id="wjglwm45"
prerequisite
→
course
```

---

# Pattern Recognition

Classic:

```txt id="wjglwm46"
Topological Sort
+
Dependency Resolution
```

Used in:

- Course Schedule I
- Course Schedule II
- Build Systems
- Package Managers
- Job Scheduling
- Dependency Graphs
