Yes — **your approach is optimal for the problem.** ✅

### Complexity

For `n` meetings:

```text
Sort meetings by finish time → O(n log n)
Greedy scan                  → O(n)
Sort selected indices        → O(n log n)
```

So overall:

```text
Time  : O(n log n)
Space : O(n)
```

The `O(n log n)` time is essentially the standard optimal comparison-based solution because the greedy algorithm needs the meetings ordered by finishing time.

### One nuance

The final:

```cpp
sort(ans.begin(), ans.end());
```

is **only because GFG requires the meeting IDs in sorted order**. It isn't part of the greedy algorithm itself.

So conceptually:

```text
Sort by finish time
        ↓
Greedy selection
        ↓
Maximum number of meetings
        ↓
Sort answer IDs
        ↓
Required GFG output format
```

Your **greedy strategy is optimal**: earliest finishing compatible meeting.

So yes, you can confidently mark this problem as **done**. 👍
