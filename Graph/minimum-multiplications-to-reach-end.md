# Minimum Multiplications to Reach End

Problem: [GFG - Minimum Multiplications to Reach End](https://www.geeksforgeeks.org/problems/minimum-multiplications-to-reach-end/1?utm_source=chatgpt.com)

---

# Problem

Given:

```cpp id="qjvwvu"
arr[]
start
end
```

Operation:

```cpp id="zjlwmv"
newNum = (current * arr[i]) % 100000
```

Find:

```txt id="wdfqv4"
minimum operations
to reach end from start
```

---

# Key Observation

Think of each number as a node.

```txt id="wv0tdd"
0
1
2
...
99999
```

Total possible states:

100000

---

# Graph Interpretation

Node:

```txt id="x2y7v8"
current number
```

Edge:

```txt id="x9v5c3"
x → (x * arr[i]) % 100000
```

for every:

```cpp id="l6w7m2"
arr[i]
```

---

# Important Insight

Each operation costs:

```txt id="wz8q1r"
1
```

Therefore every edge has weight:

```txt id="v5m2n8"
1
```

This makes it:

```txt id="n4k7d1"
Unweighted Graph
```

So use:

```txt id="b7h3q9"
BFS
```

not Dijkstra.

---

# State

Store:

```cpp id="m2q8v4"
dist[num]
```

Meaning:

```txt id="r8j6w0"
minimum operations
to reach num
```

---

# Initialization

```cpp id="h4p9k7"
dist[start] = 0;
q.push(start);
```

---

# Transition

For every multiplier:

```cpp id="z7w5m1"
next = (num * x) % 100000;
```

If:

```cpp id="f1q3n8"
dist[num] + 1 < dist[next]
```

then:

```cpp id="u9v2c6"
dist[next] = dist[num] + 1;
q.push(next);
```

---

# Why BFS Works?

BFS explores:

```txt id="y6k4p2"
distance 0
distance 1
distance 2
...
```

Since every operation costs exactly:

```txt id="c3m8r5"
1
```

the first time we reach a state is guaranteed to be with the minimum number of operations.

---

# Common Mistake

This is wrong:

```cpp id="e8r5q2"
int dist[100000] = {INT_MAX};
```

It becomes:

```txt id="a5v9w1"
dist[0] = INT_MAX
dist[1] = 0
dist[2] = 0
...
```

Use:

```cpp id="n2w6p8"
vector<int> dist(100000, INT_MAX);
```

or

```cpp id="j7c4x9"
fill(dist, dist + N, INT_MAX);
```

---

# Complexity Derivation

Let:

```cpp id="p8m3v6"
m = arr.size()
```

---

### Number of States

Possible numbers:

```txt id="w4k9d7"
0 ... 99999
```

So:

V=100000

---

### Number of Transitions

From each state we try:

```txt id="t6q2n5"
m multipliers
```

So:

E\le 100000\cdot m

---

### BFS Complexity

General BFS:

O(V+E)

Substituting:

O(100000+100000m)

Simplifies to:

O(100000m)

---

# Space Complexity

Distance array:

```txt id="r4j7n2"
100000
```

Queue:

```txt id="k8w5m3"
up to 100000 states
```

So:

O(100000)

---

# Pattern Recognition

When you see:

```txt id="q7m2v4"
minimum operations
number transformation
mod arithmetic
```

Think:

```txt id="d5k8n1"
State = number
Operation = edge
```

Then ask:

### All operations cost 1?

```txt id="u3p7r9"
Yes
→ BFS
```

### Different operation costs?

```txt id="m6q4w8"
→ Dijkstra
```

---

# Key Takeaways

1. Model each number as a graph node.
2. Generate neighbors using:

```cpp id="c9v2m5"
(num * arr[i]) % 100000
```

3. All edges have weight:

```txt id="y8n4k7"
1
```

so BFS is sufficient. 4. Complexity:

O(100000\cdot |arr|)

5. This is an **implicit graph BFS** problem.
