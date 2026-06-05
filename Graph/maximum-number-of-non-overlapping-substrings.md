# 1520. Maximum Number of Non-Overlapping Substrings

## Key Observation

If a substring contains character `c`, it must contain:

```txt
ALL occurrences of c
```

Therefore every character defines an interval:

```cpp
[first[c], last[c]]
```

---

# Step 1: First and Last Occurrence

```cpp
first[c]
last[c]
```

Example:

```txt
adefaddaccc

a -> [0,7]
d -> [1,6]
e -> [2,2]
f -> [3,3]
c -> [8,10]
```

---

# Step 2: Build Minimal Valid Interval

Start with:

```cpp
start = first[c];
end   = last[c];
```

Scan the interval.

For every character inside:

```cpp
end = max(end, last[ch]);
```

This expands the interval to include all occurrences.

---

# Invalid Interval Condition

If during scanning:

```cpp
first[ch] < start
```

then interval is invalid.

Reason:

```txt
A valid substring containing ch
must start before 'start'.
```

So this interval can never be valid.

---

# Expansion Pattern

```cpp
for(int i = start; i <= end; i++){

    int ch = s[i] - 'a';

    if(first[ch] < start)
        invalid;

    end = max(end, last[ch]);
}
```

Important:

```cpp
i <= end
```

NOT

```cpp
i < originalEnd
```

because `end` can grow during traversal.

---

# Candidate Intervals

Generate one interval per character:

```cpp
for(c = 0; c < 26; c++)
```

Maximum:

```txt
26 intervals
```

---

# Final Step = Interval Scheduling

Sort by:

```cpp
right endpoint
```

```cpp
sort(intervals.begin(), intervals.end(),
     by ending index);
```

---

# Greedy Choice

Take interval if:

```cpp
start > previousEnd
```

```cpp
if(l > lastEnd){

    take interval;

    lastEnd = r;
}
```

---

# Why Greedy Works

Choosing the interval that ends earliest:

```txt
Leaves maximum room
for future intervals.
```

Same pattern as:

- Activity Selection
- Maximum Non-overlapping Intervals

---

# Complexity

## Build First/Last

```txt
O(n)
```

---

## Build Intervals

```txt
O(26 * n)
```

---

## Sort

```txt
O(26 log 26)
```

---

## Total

```txt
O(n)
```

Space:

```txt
O(26)
```

---

# Common Mistakes

❌ Build interval for every index

```cpp
for(int i = 0; i < n; i++)
```

Causes repeated work.

Use:

```cpp
for(int c = 0; c < 26; c++)
```

---

❌ Use:

```cpp
for(i < end)
```

while `end` is expanding.

Use:

```cpp
for(i <= end)
```

---

❌ Forget invalid condition:

```cpp
first[ch] < start
```

---

# Pattern Recognition

Whenever you see:

```txt
Character must include all occurrences
```

Think:

```txt
First occurrence
Last occurrence
Interval expansion
```

Often followed by:

```txt
Greedy interval scheduling
```
