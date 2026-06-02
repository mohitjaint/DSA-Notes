# Accounts Merge

Problem: [LeetCode 721 - Accounts Merge](https://leetcode.com/problems/accounts-merge/description/?utm_source=chatgpt.com)

---

# Problem

Each account has:

```txt
Name
Email1
Email2
...
```

Two accounts belong to the same person if:

```txt
They share at least one email
```

Merge all such accounts.

---

# Key Observation

The person name is **not** useful for merging.

Example:

```txt
John
a@gmail.com

John
b@gmail.com
```

These may be different people.

---

What actually connects accounts?

```txt
Shared Email
```

---

# DSU Modeling

### DSU Node

```txt
Account Index
```

Example:

```txt
0 -> John's first account
1 -> John's second account
2 -> Mary's account
```

---

### Edge

If two accounts share an email:

```txt
Account A
↔
Account B
```

then:

```cpp
union(A, B)
```

---

# Email Mapping

Store:

```cpp
unordered_map<string, int> emailToAccount;
```

Meaning:

```txt
email
→
first account index where it appeared
```

---

Example

```txt
Account 0:
a@gmail
b@gmail

Account 1:
b@gmail
c@gmail
```

Processing:

```txt
a@gmail -> map[a] = 0
b@gmail -> map[b] = 0
```

Later:

```txt
b@gmail already exists
```

Therefore:

```cpp
union(0, 1)
```

---

# Building DSU

```cpp
for each account
    for each email

        if first occurrence:
            store email -> account

        else:
            union(
                current account,
                previous account
            )
```

---

# Grouping Emails

After all unions:

```txt
Accounts belonging to the same person
have the same DSU root
```

For every email:

```cpp
root =
findUPar(accountIndex)
```

Add email to:

```cpp
merged[root]
```

---

# My Preferred Approach

Initialize:

```cpp
vector<vector<string>>
ans(accounts.size());
```

Put account names immediately:

```cpp
for(int i = 0; i < accounts.size(); i++){

    ans[i].push_back(
        accounts[i][0]
    );
}
```

---

Then:

```cpp
for(auto &p : emailMap){

    ans[
        findUPar(p.second)
    ].push_back(p.first);
}
```

Now each root bucket contains:

```txt
Name
Email1
Email2
...
```

---

Sort emails only:

```cpp
sort(
    ans[i].begin() + 1,
    ans[i].end()
);
```

Skip index 0 because:

```txt
Index 0 = Name
```

---

Remove empty buckets:

```cpp
for(auto &vec : ans){

    if(vec.size() > 1)
        answer.push_back(vec);
}
```

---

# Why Does This Work?

Suppose:

```txt
Account 0:
a@gmail
b@gmail

Account 1:
b@gmail
c@gmail
```

Union:

```txt
0 ↔ 1
```

Now:

```txt
Root = 0
```

All emails:

```txt
a@gmail
b@gmail
c@gmail
```

get inserted into:

```txt
ans[0]
```

---

# Complexity

Let:

```txt
N = number of accounts
M = total emails
```

---

### DSU Operations

Each union/find:

O(\alpha(N))

Total:

O(M\alpha(N))

---

### Grouping Emails

Traverse map once:

```txt
O(M)
```

---

### Sorting

Suppose total emails:

```txt
M
```

Overall sorting cost:

O(M\log M)

Dominates the complexity.

---

# Final Complexity

Time:

O(M\log M)

Space:

O(M+N)

---

# DSU Concepts Used

### Path Compression

```cpp
parent[node]
=
findUPar(parent[node]);
```

---

### Union By Size

Attach smaller component under larger component.

---

### DSU Complexity

Find:

O(\alpha(N))

Union:

O(\alpha(N))

Practically:

```txt
Almost O(1)
```

---

# Pattern Recognition

### Most Stones Removed

```txt
Rows = nodes
Columns = nodes
Stone = edge
```

---

### Accounts Merge

```txt
Accounts = nodes
Shared Email = edge
```

---

# Key Takeaways

1. DSU node = **Account Index**.
2. Shared email means:

```cpp
union(account1, account2);
```

3. Use:

```cpp
email → first account
```

mapping.

4. Group emails by:

```cpp
findUPar(accountIndex)
```

5. Sort only emails, not names.

6. Complexity:

- Time: O(M\log M)
- Space: O(M+N)

7. Important DSU lesson:

```txt
Object being merged
≠
DSU node necessarily
```

The hardest part is usually identifying **what should be represented as a DSU node**. Here, accounts are nodes and emails create the connections between them.
