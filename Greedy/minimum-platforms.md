# Minimum Platforms — GFG

## Reference

**Question:** [https://www.geeksforgeeks.org/problems/minimum-platforms-1587115620/1](https://www.geeksforgeeks.org/problems/minimum-platforms-1587115620/1)

**Pattern:**
`Greedy + Sorting + Two Pointers + Event Sweep`

---

# Problem

Given:

```text
arr[i] = arrival time of train i
dep[i] = departure time of train i
```

Find the **minimum number of platforms** required so that no train has to wait.

---

# Key Observation

At any point in time, we need to know:

> **How many trains are currently present at the station?**

That number is exactly the number of platforms required at that moment.

So the answer is:

```text
maximum number of simultaneously present trains
```

---

# Greedy Idea

Sort arrivals and departures separately:

```cpp
sort(arr.begin(), arr.end());
sort(dep.begin(), dep.end());
```

Now process events chronologically.

Maintain:

```cpp
i    → next arrival
j    → next departure
cnt  → currently occupied platforms
req  → maximum platforms required
```

---

# Core Decision

Compare:

```cpp
arr[i]
```

with:

```cpp
dep[j]
```

### Case 1 — Arrival happens first

```cpp
arr[i] <= dep[j]
```

A train arrives before the earliest currently considered departure.

Therefore, we need another platform:

```cpp
cnt++;
i++;
req = max(req, cnt);
```

---

### Case 2 — Departure happens first

```cpp
arr[i] > dep[j]
```

A train has departed before the next arrival.

Its platform becomes available:

```cpp
cnt--;
j++;
```

---

# Why `<=`?

This is an important detail.

Suppose:

```text
Arrival   = 1000
Departure = 1000
```

For this problem, the arriving train still requires a separate platform because the departing train occupies its platform at that time.

Therefore:

```cpp
if(arr[i] <= dep[j])
```

not:

```cpp
if(arr[i] < dep[j])
```

---

# Example

Consider:

```text
arr = [900, 940, 950, 1100, 1500, 1800]

dep = [910, 1200, 1120, 1130, 1900, 2000]
```

Sort:

```text
arr = [900, 940, 950, 1100, 1500, 1800]

dep = [910, 1120, 1130, 1200, 1900, 2000]
```

Now process:

```text
900 <= 910
→ arrival
→ cnt = 1

940 > 910
→ departure
→ cnt = 0

940 <= 1120
→ arrival
→ cnt = 1

950 <= 1120
→ arrival
→ cnt = 2

1100 <= 1120
→ arrival
→ cnt = 3

...
```

The maximum `cnt` reached is the required number of platforms.

---

# Algorithm

```text
Sort arrivals
Sort departures

i = 0
j = 0
cnt = 0
req = 0

while i < n:

    if arr[i] <= dep[j]:

        train arrives
        cnt++
        i++

        req = max(req, cnt)

    else:

        train departs
        cnt--
        j++

return req
```

---

# Code

```cpp
class Solution {
public:
    int minPlatform(vector<int>& arr, vector<int>& dep) {
        int n = arr.size();

        sort(arr.begin(), arr.end());
        sort(dep.begin(), dep.end());

        int i = 0;
        int j = 0;
        int cnt = 0;
        int req = 0;

        while(i < n) {
            if(arr[i] <= dep[j]) {
                cnt++;
                i++;

                req = max(req, cnt);
            }
            else {
                cnt--;
                j++;
            }
        }

        return req;
    }
};
```

---

# Why This Works

After sorting, we always look at the **earliest unprocessed arrival and departure**.

There are only two possibilities:

```text
arrival <= departure
```

→ another train is present before a platform is freed.

or:

```text
arrival > departure
```

→ a platform has already become free.

Thus `cnt` always represents the number of trains currently requiring platforms.

The maximum value of `cnt` is therefore the minimum number of platforms needed.

---

# Complexity

Sorting:

```text
O(n log n)
```

Two-pointer scan:

```text
O(n)
```

Therefore:

```text
Time  : O(n log n)
Space : O(1) extra
```

---

# Common Mistakes

### 1. Not sorting

Without sorting, you cannot process arrivals/departures chronologically.

---

### 2. Using `<` instead of `<=`

For this problem:

```cpp
arr[i] <= dep[j]
```

is required because simultaneous arrival and departure require separate platforms.

---

### 3. Forgetting to update the maximum

The final `cnt` is **not necessarily** the answer.

We need the maximum number of simultaneously occupied platforms:

```cpp
req = max(req, cnt);
```

---

### 4. Confusing this with Activity Selection

Both involve intervals, but the goals are different.

### Activity Selection

```text
Maximum number of non-overlapping meetings
        ↓
Sort by finish time
```

### Minimum Platforms

```text
Maximum simultaneous intervals
        ↓
Sort arrivals + departures
        ↓
Sweep through events
```

---

# Pattern Recognition

When you see:

```text
Intervals

+

How many resources are needed simultaneously?

+

Each interval occupies one resource
```

think:

```text
Sort start events
Sort end events
        ↓
Two pointers / sweep
        ↓
Track active intervals
        ↓
Take maximum
```

Examples:

- Minimum Platforms
- Meeting Rooms II
- CPU/resource allocation
- Maximum simultaneous events

---

# Interview Trigger

```text
Need minimum resources
for overlapping intervals

        ↓

Sort start times
Sort end times

        ↓

Compare earliest start/end

        ↓

Start <= end
    → resource needed

End < start
    → resource freed

        ↓

Track maximum active resources
```

---

# Core Insight

> **The answer is the maximum number of trains present at the same time.**

Sorting arrivals and departures lets us simulate the station chronologically without explicitly creating every interval.

```text
Arrival
   ↓
+1 platform

Departure
   ↓
-1 platform

Maximum active count
   ↓
Minimum required platforms
```

This is the classic **interval sweep-line greedy pattern**.
