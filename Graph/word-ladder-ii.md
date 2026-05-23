# Word Ladder II

Problem : [LeetCode - Word Ladder II](https://leetcode.com/problems/word-ladder-ii/description/?utm_source=chatgpt.com)

---

## Problem

Given:

- `beginWord`
- `endWord`
- `wordList`

Transform:

```txt id="p4v43g"
beginWord → endWord
```

Rules:

- Change only **one character** at a time
- New word must exist in dictionary
- Return **ALL shortest transformation sequences**

Example:

```txt id="h0l7ra"
hit
↓

hot
↓

dot lot
↓

dog log
↓

cog
```

Output:

```txt id="of5u3z"
[
 [hit hot dot dog cog],
 [hit hot lot log cog]
]
```

---

# Key Observation

This is NOT:

```txt id="e8u0od"
find one shortest path
```

This is:

```txt id="vzc9wx"
find ALL shortest paths
```

Need two things:

```txt id="snq5i8"
Shortest
+
All sequences
```

---

# Wrong Intuition

```txt id="u5r6fc"
Need paths
⇒ DFS
```

DFS alone fails.

Because:

```txt id="zvij7l"
DFS explores
long unnecessary paths
```

---

# Correct Approach

Use:

```txt id="jdnyvi"
BFS
+
DFS
```

---

## Why BFS?

BFS guarantees:

```txt id="1t0h3m"
minimum number of transformations
```

---

## Why DFS?

After shortest structure is built:

```txt id="v7u0gq"
DFS reconstructs
ALL valid sequences
```

---

# Main Insight

Do NOT store:

```txt id="lgqydj"
parent → children
```

Store:

```txt id="q6szd6"
child → parents
```

Example:

Instead of:

```txt id="gbw0yf"
hit → hot
hot → dot
hot → lot
dot → dog
lot → log
dog → cog
log → cog
```

Store:

```txt id="ew0jkr"
hot ← hit

dot ← hot
lot ← hot

dog ← dot
log ← lot

cog ← dog
cog ← log
```

Reason:

DFS starts from:

```txt id="i9q84f"
endWord
```

and moves upward.

---

# Data Structures

### Dictionary

```cpp
unordered_set<string> dict;
```

Purpose:

```txt
word existence
+
visited
```

---

### BFS Queue

```cpp
queue<string> q;
```

Purpose:

```txt
level traversal
```

---

### Level Map

```cpp
unordered_map<string,int> level;
```

Stores:

```txt
word → shortest level
```

Example:

```txt
hit → 1
hot → 2
dot → 3
dog → 4
```

---

### Parent Graph

```cpp
unordered_map<
string,
unordered_set<string>
> parents;
```

Stores:

```txt
child → all shortest parents
```

Example:

```txt
cog → {dog, log}
```

---

# BFS Construction

Push:

```cpp
q.push(beginWord);
```

Store:

```cpp
level[beginWord]=1;
```

---

For every generated word:

### First time discovered

```cpp
if(!level.count(next))
```

Store:

```cpp
level[next]
=
level[curr]+1;

parents[next]
.insert(curr);

q.push(next);
```

---

### Already discovered on same shortest level

Do NOT push again.

Only:

```cpp
parents[next]
.insert(curr);
```

---

# Important Trick

Delete words:

```txt
after entire level
```

NOT immediately.

Reason:

Example:

```txt
dog → cog
log → cog
```

Both paths must survive.

---

Use:

```cpp
unordered_set<string>
toDelete;
```

After level:

```cpp
for(auto& word:toDelete)
    dict.erase(word);
```

---

# DFS Reconstruction

Start:

```cpp
path = {endWord}
```

Move:

```txt
endWord
↑
parents
↑
beginWord
```

Backtracking:

```cpp
path.push_back(parent);

dfs(...);

path.pop_back();
```

When:

```cpp
level[word]==1
```

Reverse path.

Store answer.

---

# Final Solution

```cpp
class Solution {
public:

    void dfs(
        string word,
        unordered_map<
            string,
            unordered_set<string>
        >& parents,

        unordered_map<
            string,
            int
        >& level,

        vector<string>& path,

        vector<vector<string>>& ans
    ){

        if(level[word]==1){

            reverse(
                path.begin(),
                path.end()
            );

            ans.push_back(path);

            reverse(
                path.begin(),
                path.end()
            );

            return;
        }

        for(
            auto& parent
            :
            parents[word]
        ){

            if(
                level[word]
                -
                level[parent]
                ==
                1
            ){

                path.push_back(parent);

                dfs(
                    parent,
                    parents,
                    level,
                    path,
                    ans
                );

                path.pop_back();
            }
        }
    }

    vector<vector<string>>
    findLadders(
        string beginWord,
        string endWord,
        vector<string>& wordList
    ){

        unordered_set<string>
        dict(
            wordList.begin(),
            wordList.end()
        );

        queue<string> q;

        unordered_map<
            string,
            int
        > level;

        unordered_map<
            string,
            unordered_set<string>
        > parents;

        q.push(beginWord);

        level[beginWord]=1;

        dict.erase(beginWord);

        bool found=false;

        while(
            !q.empty()
        ){

            if(found)
                break;

            unordered_set<
                string
            > toDelete;

            int size=q.size();

            while(size--){

                string word=
                    q.front();

                q.pop();

                if(
                    word
                    ==
                    endWord
                ){

                    found=true;

                    continue;
                }

                for(
                    int i=0;
                    i<word.size();
                    i++
                ){

                    string parent=
                        word;

                    for(
                        char c='a';
                        c<='z';
                        c++
                    ){

                        if(
                            c
                            ==
                            parent[i]
                        )
                            continue;

                        word[i]=c;

                        if(
                            dict.count(
                                word
                            )
                        ){

                            if(
                                !level.count(
                                    word
                                )
                            ){

                                level[word]
                                =
                                level[parent]
                                +
                                1;

                                if(
                                    !toDelete
                                    .count(
                                        word
                                    )
                                ){

                                    q.push(
                                        word
                                    );
                                }
                            }

                            toDelete
                            .insert(
                                word
                            );

                            parents[word]
                            .insert(
                                parent
                            );
                        }

                        word=parent;
                    }
                }
            }

            for(
                auto& word
                :
                toDelete
            ){

                dict.erase(word);
            }
        }

        vector<
            vector<string>
        > ans;

        if(
            !level.count(
                endWord
            )
        )
            return ans;

        vector<string>
            path
            =
            {
                endWord
            };

        dfs(
            endWord,
            parents,
            level,
            path,
            ans
        );

        return ans;
    }
};
```

---

# Complexity

Let:

- `N` = number of words
- `L` = word length

BFS:

O(NL\cdot26)

DFS:

Depends on output size.

Overall:

```txt
BFS
+
Output reconstruction
```

---

# Common Mistakes

❌ DFS only

---

❌ Delete immediately

---

❌ Store neighbors instead of parents

---

❌ Break middle of BFS level

---

❌ Push same word multiple times

---

# Pattern Recognition

Classic:

```txt
Shortest Path
+
Path Reconstruction
```

Used in:

- Word Ladder II
- All Shortest Paths
- Route Reconstruction
- DAG Path Enumeration
