# LFU Cache

## References

**Question:**

LeetCode 460 — LFU Cache

**Related Problems:**

- LRU Cache — LeetCode 146
- All O(1) Data Structure — LeetCode 432
- Design HashMap

---

# Pattern

```text
HashMap
+
Doubly Linked List
+
Frequency Buckets
```

---

# Problem Statement

Design an **LFU (Least Frequently Used) Cache** supporting:

```cpp
get(key)
put(key, value)
```

Both operations should be **O(1) average**.

When the cache is full, remove the:

```text
Least Frequently Used
```

key.

If multiple keys have the same frequency, remove the:

```text
Least Recently Used
```

key among them.

---

# Why LRU Is Not Enough

In LRU, we only care about:

```text
How recently was this key used?
```

In LFU, we need:

```text
How many times was this key used?
```

and if frequencies are equal:

```text
Which one was used least recently?
```

Therefore, we need **two levels of organization**.

---

# Data Structures

## 1. HashMap: `keyNode`

```cpp
unordered_map<int, Node*> keyNode;
```

Maps:

```text
key
 ↓
Node*
```

This gives direct access to a key's node.

---

## 2. Frequency → Doubly Linked List

```cpp
unordered_map<int, List*> freqListMap;
```

Maps:

```text
frequency
    ↓
DLL containing all nodes with that frequency
```

Example:

```text
freq = 1
A → B → C

freq = 2
D → E

freq = 3
F
```

Each frequency list maintains **LRU order** among nodes having that frequency.

---

## 3. `minFreq`

```cpp
int minFreq;
```

Stores the **minimum frequency currently present in the cache**.

This lets us immediately know which frequency bucket to evict from.

---

# Node

Each node stores:

```cpp
struct Node {
    int key;
    int val;
    int cnt;
    Node* prev;
    Node* next;
};
```

Where:

```text
key → cache key
val → cache value
cnt → frequency of usage
prev/next → DLL pointers
```

Every new node starts with:

```text
cnt = 1
```

---

# Frequency List

Each frequency has a DLL:

```text
head
 ↓
Most Recently Used
 ↓
...
 ↓
Least Recently Used
 ↓
tail
```

Dummy `head` and `tail` nodes simplify insertion and deletion.

---

# Why Multiple DLLs?

Suppose:

```text
Frequency 1:

A → B


Frequency 2:

C → D


Frequency 3:

E
```

If

```text
minFreq = 1
```

we only consider:

```text
A, B
```

for eviction.

Since `B` is the least recently used inside frequency 1,

```text
B
```

is evicted.

This implements:

```text
LFU
 ↓
LRU tie-breaker
```

---

# `addFront()`

Adds a node immediately after the dummy head.

```text
Before:

head → A → B → tail

After adding C:

head → C → A → B → tail
```

Therefore, the front is always:

```text
Most Recently Used
```

---

# `deleteNode()`

Removes an existing node from its DLL.

```text
A ↔ B ↔ C

Remove B

↓

A ↔ C
```

Only pointer changes are required.

```text
O(1)
```

Important:

> `deleteNode()` should only unlink the node. It should not `delete` the node unless the node is being permanently evicted.

---

# `updateFreqListMap(node)`

This is the **core operation**.

Whenever a node is accessed:

```text
frequency++

```

### Steps

```text
Remove node from current frequency list

↓

If current frequency was minFreq
and its list becomes empty,
increase minFreq

↓

Increase node->cnt

↓

Move node into next frequency list

↓

Make it MRU in that list
```

---

# Example

Suppose:

```text
Frequency 2:

A → B → C
```

and:

```text
get(B)
```

Then:

```text
Frequency 2:

A → C


Frequency 3:

B
```

`B` has moved from frequency 2 to frequency 3.

---

# Important `minFreq` Case

Suppose:

```text
minFreq = 2

Frequency 2:

A
```

Now:

```text
get(A)
```

A moves to frequency 3:

```text
Frequency 2:

empty

Frequency 3:

A
```

Therefore:

```cpp
minFreq = 3;
```

This is why we check:

```cpp
if(node->cnt == minFreq &&
   freqListMap[node->cnt]->size == 0)
{
    minFreq++;
}
```

---

# `get(key)`

### If key doesn't exist

```cpp
return -1;
```

### If key exists

```text
Find node using keyNode

↓

Increase frequency

↓

Move node to corresponding frequency DLL

↓

Return value
```

---

# `put(key, value)`

## Case 1 — Key already exists

```text
Find node

↓

Update value

↓

Increase frequency

↓

Move node to next frequency list
```

Important:

> Updating an existing key also counts as using the key.

---

## Case 2 — New key

If:

```text
capacity == 0
```

return immediately.

Otherwise, if cache is full:

```text
Find freqListMap[minFreq]

↓

Take tail->prev

↓

This is the LRU node
within the minimum frequency

↓

Remove from HashMap

↓

Remove from DLL

↓

Delete node
```

Then:

```text
Create new node

↓

cnt = 1

↓

minFreq = 1

↓

Insert into frequency-1 list

↓

Add to keyNode
```

---

# Eviction Rule

Always remember:

```text
First priority:
Minimum frequency

Second priority:
Least recently used
```

In other words:

```text
LFU
 ↓
if tie
 ↓
LRU
```

---

# Example

Capacity:

```text
2
```

Perform:

```text
put(1,10)
put(2,20)
```

Both have frequency 1:

```text
freq 1:

2 → 1
```

assuming `2` was inserted more recently.

Now:

```text
get(1)
```

Frequency becomes:

```text
freq 1:

2

freq 2:

1
```

Now:

```text
put(3,30)
```

Minimum frequency is:

```text
1
```

So we evict from frequency 1:

```text
2
```

Result:

```text
freq 1:

3

freq 2:

1
```

---

# Complexity

Using `unordered_map`:

```text
get() → O(1) average
put() → O(1) average
```

Why?

```text
HashMap lookup       → O(1) average
DLL insertion        → O(1)
DLL deletion         → O(1)
Frequency update     → O(1)
Eviction             → O(1)
```

Space:

```text
O(capacity)
```

---

# Important Implementation Detail

Use:

```cpp
unordered_map<int, Node*> keyNode;
unordered_map<int, List*> freqListMap;
```

rather than:

```cpp
map<int, Node*>
map<int, List*>
```

because:

```text
map           → O(log n)
unordered_map → O(1) average
```

If the requirement is O(1) average, `unordered_map` is the appropriate choice.

---

# Common Mistake #1

Using only one DLL.

One DLL can maintain LRU order, but it cannot efficiently tell us:

```text
Which frequency should be evicted?
```

We need:

```text
frequency → DLL
```

---

# Common Mistake #2

Forgetting the LRU tie-breaker.

LFU does **not** simply mean:

```text
Remove any node with minimum frequency.
```

It means:

```text
Minimum frequency

+

Least recently used among those nodes
```

---

# Common Mistake #3

Forgetting to update `minFreq`.

If the entire minimum-frequency list becomes empty after an access:

```text
minFreq
```

must move to the next frequency.

---

# Common Mistake #4

Not resetting `minFreq` when inserting a new node.

Every new node has:

```text
cnt = 1
```

Therefore:

```cpp
minFreq = 1;
```

whenever a new key is inserted.

---

# Common Mistake #5

Deleting the node inside `deleteNode()`.

Don't do:

```cpp
deleteNode(node);
delete node;
```

inside the helper itself.

The helper may be used when merely **moving** a node between frequency lists.

Only permanently evicted nodes should be freed.

---

# LRU vs LFU

|                 | LRU           | LFU                       |
| --------------- | ------------- | ------------------------- |
| Main criterion  | Recency       | Frequency                 |
| Tie breaker     | —             | Recency                   |
| DLLs            | 1             | Multiple                  |
| HashMap         | key → node    | key → node                |
| Extra structure | —             | frequency → DLL           |
| Minimum tracker | —             | `minFreq`                 |
| `get()`         | Move to front | Increase frequency + move |
| Eviction        | Tail          | Tail of `minFreq` list    |

---

# Pattern Recognition

When a problem says:

```text
Cache

+

Remove least frequently used

+

Tie → least recently used

+

O(1) operations
```

think:

```text
HashMap
+
Frequency Buckets
+
Doubly Linked Lists
+
minFreq
```

---

# Interview Trigger

```text
LFU Cache

↓

Need O(1) key lookup

↓

HashMap: key → Node

↓

Need frequency tracking

↓

Node stores frequency

↓

Need nodes grouped by frequency

↓

frequency → Doubly Linked List

↓

Need fastest eviction

↓

minFreq

↓

Evict tail->prev of minFreq list

↓

New node starts at frequency 1

↓

O(1) average
```

---

# Core Insight

**LRU uses one ordering; LFU needs two.**

For LRU:

```text
key → node
       ↓
one DLL ordered by recency
```

For LFU:

```text
key → node
       ↓
node stores frequency
       ↓
frequency → DLL
       ↓
each DLL maintains LRU order
```

`minFreq` tells us **which frequency bucket to inspect**, while the tail of that bucket tells us **which node to evict**.

So the complete mental model is:

```text
                  LFU Cache
                     │
          ┌──────────┴──────────┐
          ↓                     ↓
     key → Node            minFreq
          │                     │
          ↓                     ↓
       frequency          frequency DLL
                                │
                     ┌──────────┼──────────┐
                     ↓          ↓          ↓
                   freq 1     freq 2     freq 3
                     │          │          │
                    DLL        DLL        DLL
                     │
                LRU order
```

**LRU:** _Who was used least recently?_

**LFU:** _Who was used least frequently? If tied, who was used least recently?_
