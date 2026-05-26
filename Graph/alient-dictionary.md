# Alien Dictionary — Topological Sort (Kahn's Algorithm)

Problem : [GFG - Alien Dictionary](https://www.geeksforgeeks.org/problems/alien-dictionary/1?utm_source=chatgpt.com)

---

# Problem

Given:

```txt id="jlwm1"
dictionary of words
sorted according to
alien language
```

Need to find:

```txt id="wjglwm2"
order of characters
```

Example:

Input:

```txt id="wjglwm3"
baa
abcd
abca
cab
cad
```

Output:

```txt id="wjglwm4"
bdac
```

Meaning:

```txt id="wjglwm5"
b before d
d before a
a before c
```

---

# Key Observation

Words are already sorted.

Compare:

```txt id="wjglwm6"
adjacent words only
```

because:

```txt id="wjglwm7"
dictionary order
already gives constraints
```

---

Example

```txt id="wjglwm8"
abcd
abed
```

Compare:

```txt id="wjglwm9"
a == a
b == b

c ≠ e
```

First mismatch:

```txt id="wjglwm10"
c → e
```

That means:

```txt id="wjglwm11"
c comes before e
```

Create edge:

```txt id="wjglwm12"
c → e
```

Stop comparison.

---

# Why First Difference Only?

Example:

```txt id="wjglwm13"
abcd
abef
```

Only:

```txt id="wjglwm14"
c → e
```

matters.

Do NOT use:

```txt id="wjglwm15"
d → f
```

because order already decided earlier.

---

# Graph Construction

Node:

```txt id="wjglwm16"
character
```

Edge:

```txt id="wjglwm17"
u → v
```

means:

```txt id="wjglwm18"
u comes before v
```

Store:

```cpp id="wjglwm19"
adj[u].push_back(v);
indegree[v]++;
```

---

# Algorithm

### Step 1

Compare adjacent words.

Find:

```txt id="wjglwm20"
first different character
```

Build graph.

---

### Step 2

Compute:

```txt id="wjglwm21"
indegree
```

Meaning:

```txt id="wjglwm22"
how many characters
must come before
```

---

### Step 3

Push:

```txt id="wjglwm23"
indegree==0
```

into queue.

---

### Step 4

Run Kahn's Algorithm.

Take:

```txt id="wjglwm24"
current character
```

Append to answer.

Reduce:

```cpp id="wjglwm25"
indegree[child]--
```

Push if:

```txt id="wjglwm26"
indegree==0
```

---

# Visual Example

Input:

```txt id="wjglwm27"
baa
abcd
abca
cab
cad
```

Compare:

```txt id="wjglwm28"
baa
abcd
```

Difference:

```txt id="wjglwm29"
b → a
```

---

Compare:

```txt id="wjglwm30"
abcd
abca
```

Difference:

```txt id="wjglwm31"
d → a
```

---

Compare:

```txt id="wjglwm32"
abca
cab
```

Difference:

```txt id="wjglwm33"
a → c
```

---

Compare:

```txt id="wjglwm34"
cab
cad
```

Difference:

```txt id="wjglwm35"
b → d
```

Graph:

```txt id="wjglwm36"
b → d
↓

a → c
```

Topological order:

```txt id="wjglwm37"
bdac
```

---

# Invalid Prefix Case

Example:

```txt id="wjglwm38"
abcd
ab
```

Impossible.

Because:

```txt id="wjglwm39"
longer word
cannot come before
its prefix
```

Handle:

```cpp id="wjglwm40"
if(
j==dict[i+1].size()
&&
dict[i].size()
>
dict[i+1].size()
)
return "";
```

---

# Final Code

```cpp id="wjglwm41"
class Solution {
public:
    string findOrder(string dict[], int N, int K) {

        vector<vector<int>> adj(K);
        vector<int> indegree(K, 0);

        for (int i = 0; i < N - 1; i++) {

            int j = 0;

            while (
                j < dict[i].size() &&
                j < dict[i + 1].size()
            ) {

                if (dict[i][j] != dict[i + 1][j]) {

                    int u = dict[i][j] - 'a';
                    int v = dict[i + 1][j] - 'a';

                    adj[u].push_back(v);
                    indegree[v]++;

                    break;
                }

                j++;
            }

            if (
                j == dict[i + 1].size() &&
                dict[i].size() > dict[i + 1].size()
            ) {
                return "";
            }
        }

        queue<int> q;

        for (int i = 0; i < K; i++) {
            if (indegree[i] == 0) {
                q.push(i);
            }
        }

        string ans;

        while (!q.empty()) {

            int node = q.front();
            q.pop();

            ans.push_back(node + 'a');

            for (int nei : adj[node]) {

                indegree[nei]--;

                if (indegree[nei] == 0) {
                    q.push(nei);
                }
            }
        }

        return ans;
    }
};
```

---

# Complexity

Let:

- `N` = number of words
- `L` = average word length
- `K` = alphabet size

Time:

O(N\cdot L+K)

---

Space:

O(K+E)

---

# Pattern Recognition

When you see:

```txt id="wjglwm42"
order
dictionary
dependencies
precedence
```

Think:

```txt id="wjglwm43"
Graph
+
Topological Sort
```

And the important trick:

```txt id="wjglwm44"
first differing character
=
edge
```
