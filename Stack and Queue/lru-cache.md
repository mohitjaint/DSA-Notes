# LRU Cache (LeetCode 146)

## References

**Question:**

[https://leetcode.com/problems/lru-cache/](https://leetcode.com/problems/lru-cache/)

**Related Problems:**

- LFU Cache
- All O`(1)` Data Structure (LeetCode 432)
- Design Browser History
- Design HashMap

---

# Pattern

```text
Hash Map

+

Doubly Linked List

+

Design
```

---

# Problem Statement

Design an LRU (Least Recently Used) cache supporting

```text
get(key)

put(key, value)
```

Both operations must run in

```text
O(1)
```

time.

---

# Brute Force

Store all elements in a list.

For every

```text
get()
```

search the list.

Move the element to the front.

For

```text
put()
```

search again.

If full,

remove the least recently used element.

---

## Complexity

```text
Time  : O(n)

Space : O(capacity)
```

Not acceptable.

---

# Key Observation

We need **two different operations**.

## 1. Find a key quickly

Need

```text
key

↓

node
```

This is exactly what

```text
unordered_map
```

provides.

---

## 2. Move an element to the front

Need

```text
Remove from middle

↓

Insert at front
```

Both must be

```text
O(1)
```

Array, vector and deque cannot remove arbitrary elements in O(1).

A **Doubly Linked List** can.

---

# Why Not Deque?

Suppose

```text
Front

A B C D E

Back
```

Access

```text
C
```

LRU requires

```text
A B D E C
```

A deque cannot remove

```text
C
```

from the middle in

```text
O(1)
```

A doubly linked list can.

---

# Data Structures

## Hash Map

```text
key

↓

Node*
```

Provides

```text
O(1)
```

lookup.

---

## Doubly Linked List

Stores cache entries in usage order.

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

Dummy head and tail simplify insertion and deletion.

---

# Node Structure

Each node stores

```text
Key

Value

Previous Pointer

Next Pointer
```

---

# Helper Functions

## Insert After Head

Marks a node as

```text
Most Recently Used
```

```text
head

↓

node

↓

old first node
```

---

## Delete Node

Removes a node from the linked list.

Only changes pointers.

**Does NOT free memory.**

```text
prev <-> node <-> next

↓

prev <-------> next
```

---

## Move To Front

```cpp
deleteNode(node);

insertAfterHead(node);
```

Used by both

```text
get()

put(existing)
```

---

# Get Operation

If

```text
Key not found
```

Return

```text
-1
```

Otherwise

```text
Find node

↓

Move to front

↓

Return value
```

---

# Put Operation

---

## Case 1

Key already exists.

```text
Find node

↓

Update value

↓

Move to front
```

---

## Case 2

New key.

If cache is full

```text
Least Recently Used

↓

tail->prev
```

Remove

```text
HashMap entry

↓

Delete from DLL

↓

delete node
```

Then

```text
Create new node

↓

Insert after head

↓

Add to HashMap
```

---

# Algorithm

```text
HashMap

↓

O(1) lookup

↓

DLL

↓

O(1) delete

↓

O(1) insert

↓

Maintain MRU at head

↓

Maintain LRU at tail
```

---

# Example

Capacity

```text
2
```

---

```text
put(1,10)

head

↓

1

↓

tail
```

---

```text
put(2,20)

head

↓

2

↓

1

↓

tail
```

---

```text
get(1)
```

Move

```text
1
```

to front

```text
head

↓

1

↓

2

↓

tail
```

Return

```text
10
```

---

```text
put(3,30)
```

Cache full.

Remove

```text
tail->prev

↓

2
```

Final

```text
head

↓

3

↓

1

↓

tail
```

---

# Why Dummy Nodes?

Without dummy nodes

every insertion/removal must check

```text
Empty list?

First node?

Last node?
```

With

```text
head

↓

...

↓

tail
```

every insertion and deletion becomes identical.

No special cases.

---

# Why It Works

The HashMap always knows where every node is.

The Doubly Linked List always maintains usage order.

Whenever a key is accessed,

it becomes the

```text
Most Recently Used
```

Whenever the cache becomes full,

the node before

```text
tail
```

is always the

```text
Least Recently Used
```

---

# Complexity

| Operation | Time |
| --------- | ---- |
| get       | O(1) |
| put       | O(1) |
| insert    | O(1) |
| delete    | O(1) |

Space

```text
O(capacity)
```

---

# Common Mistake #1

Using a deque.

Although deque supports

```text
push_front

push_back

pop_front

pop_back
```

it **cannot remove an arbitrary middle element in O(1)**.

---

# Common Mistake #2

Deleting the node inside

```cpp
deleteNode()
```

Wrong

```cpp
deleteNode(node);

delete node;
```

`deleteNode()` should **only unlink** the node.

Memory should be freed **only when the node is permanently removed** from the cache.

---

# Common Mistake #3

Forgetting to move a node after

```text
get()
```

Every successful access makes the node

```text
Most Recently Used
```

---

# Common Mistake #4

Updating an existing key by creating a new node.

Simply

```text
Update value

↓

Move existing node to front
```

---

# Common Mistake #5

Not handling

```text
capacity = 0
```

No node should ever be inserted.

---

# Common Mistake #6

Forgetting to remove the evicted key from the HashMap.

Always perform

```text
HashMap erase

↓

Remove from DLL

↓

delete node
```

---

# Comparison

| Data Structure     | Lookup |       Remove Middle | Insert Front |
| ------------------ | -----: | ------------------: | -----------: |
| Array              |   O(n) |                O(n) |         O(n) |
| Vector             |   O(n) |                O(n) |         O(n) |
| Deque              |   O(n) |                O(n) |         O(1) |
| Doubly Linked List |   O(n) | O(1) (with pointer) |         O(1) |
| HashMap + DLL      |   O(1) |                O(1) |         O(1) |

---

# Pattern Recognition

Whenever you see

```text
Need O(1) lookup

+

Need O(1) delete from middle

+

Need O(1) insert
```

think

```text
HashMap

+

Doubly Linked List
```

This combination appears in many design problems.

---

# Interview Trigger

```text
Need LRU Cache

↓

Need O(1) lookup

↓

HashMap

↓

Need O(1) move to front

↓

Doubly Linked List

↓

MRU near head

↓

LRU near tail

↓

get()

↓

Move to front

↓

put()

↓

Evict tail->prev if full

↓

O(1)
```

---

# Core Insight

The problem requires **two independent capabilities**: fast lookup by key and fast reordering by recent usage. A **HashMap** provides direct access from a key to its corresponding node, while a **Doubly Linked List** allows that node to be removed and reinserted at the front in constant time. By maintaining the **Most Recently Used (MRU)** node immediately after the dummy head and the **Least Recently Used (LRU)** node immediately before the dummy tail, both `get()` and `put()` operations can be implemented in **O(1)** time. The key design principle is to **separate responsibilities**:

- **HashMap** → locate nodes.
- **Doubly Linked List** → maintain usage order.
- **`deleteNode()`** → only unlinks nodes.
- **`moveToFront()`** → marks a node as recently used.
- **`delete node`** → only when evicting an entry permanently.
