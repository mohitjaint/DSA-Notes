# Word Ladder

Problem : [LeetCode - Word Ladder](https://leetcode.com/problems/word-ladder/description/?utm_source=chatgpt.com)

---

## Problem

Given:

- `beginWord`
- `endWord`
- `wordList`

Transform:

```txt
beginWord → endWord
```

Rules:

- Change exactly **one character** at a time
- New word must exist in `wordList`
- Return **minimum number of transformations**

If impossible:

```txt
return 0
```

---

## Graph Interpretation

Treat:

```txt
word = node
```

Connect two words if:

```txt
they differ by exactly one character
```

Example:

```txt
hit
↓
hot
↓
dot
↓
dog
↓
cog
```

Question becomes:

```txt
Find shortest path
in an unweighted graph
```

---

## Key Observation

All transformations cost:

```txt
1 step
```

Shortest path in unweighted graph:

```txt
→ BFS
```

DFS is not suitable because it explores unnecessary longer paths.

---

## Wrong Approach (Initial Idea)

Build full graph:

```txt
compare every pair of words
```

Then:

```txt
DFS all paths
```

Problems:

- expensive graph construction
- explores too many paths
- shortest path not guaranteed

Complexity:

O(N^2L)

Too slow.

---

## Core Insight

Do NOT build graph.

Generate neighbors dynamically.

For current word:

```txt
hot

aot
bot
...
dot
...
hoz
```

Only keep generated words that exist in dictionary.

---

## Data Structures

### Dictionary + Visited

```cpp
unordered_set<string> st;
```

Stores:

- available words
- visited state

After visiting:

```cpp
st.erase(word);
```

No separate visited set needed.

---

### BFS Queue

```cpp
queue<string> q;
```

Stores:

```txt
current BFS layer
```

---

## Algorithm

### Step 1

Store all words in:

```cpp
unordered_set
```

---

### Step 2

Push:

```cpp
beginWord
```

into queue.

Remove from set.

---

### Step 3

Run BFS level-by-level.

For every word:

- choose one position
- replace with `a → z`
- generate new word

---

### Step 4

If generated word exists:

```cpp
push
erase
```

---

### Step 5

First time reaching:

```cpp
endWord
```

return level.

---

## Optimal Solution

```cpp
class Solution {
public:
    int ladderLength(
        string beginWord,
        string endWord,
        vector<string>& wordList
    ) {
        unordered_set<string> st(
            wordList.begin(),
            wordList.end()
        );

        queue<string> q;

        q.push(beginWord);
        st.erase(beginWord);

        int level = 0;

        while (!q.empty()) {
            level++;

            int size = q.size();

            while (size--) {
                string word = q.front();
                q.pop();

                if (word == endWord)
                    return level;

                for (int i = 0; i < word.size(); i++) {
                    for (char c = 'a'; c <= 'z'; c++) {
                        string newWord = word;

                        newWord[i] = c;

                        if (st.find(newWord) != st.end()) {
                            st.erase(newWord);
                            q.push(newWord);
                        }
                    }
                }
            }
        }

        return 0;
    }
};
```

---

## Complexity

Let:

- `N` = number of words
- `L` = word length

For every word:

```txt
L × 26 transformations
```

Time:

O(NL\cdot26)

Simplifies to:

O(NL)

---

Space:

Queue + Set:

O(N)

Optimal.

---

## Why Erase From Set?

Instead of:

```cpp
visited.insert(word)
```

Use:

```cpp
st.erase(word)
```

Reason:

```txt
visited word
=
never needed again
```

Saves memory.

---

## Common Mistakes

❌ Using DFS for shortest path

---

❌ Building complete graph

---

❌ Increasing level per node

Correct:

```txt
increase per BFS layer
```

---

❌ Keeping `cout`

Can cause TLE.

---

❌ Forgetting to remove visited words

Leads to repeated processing.

---

## Important Insight

This problem teaches:

```txt
Implicit Graph
+
Shortest Path
+
BFS
+
State Generation
```

---

## Pattern Recognition

Classic pattern:

```txt
Generate State
+
BFS
```

Used in:

- Word Ladder
- Open the Lock
- Minimum Genetic Mutation
- Sliding Puzzle
- State Space Search
