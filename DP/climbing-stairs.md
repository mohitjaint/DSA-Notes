# DP 01: Climbing Stairs

## Problem

You can climb:

```txt id="88xjlwm"
1 step
or
2 steps
```

Find the number of ways to reach step `n`.

---

# State Definition

```cpp id="gwjlwm"
dp[i]
```

=

```txt id="jlwm11"
Number of ways to reach step i
```

---

# Recurrence

To reach step `i`:

### Take 1 step

Come from:

```cpp id="jlwm22"
i - 1
```

### Take 2 steps

Come from:

```cpp id="jlwm33"
i - 2
```

Therefore:

dp[i]=dp[i-1]+dp[i-2]

---

# Base Cases

```cpp id="jlwm44"
dp[0] = 1
dp[1] = 1
```

Meaning:

```txt id="jlwm55"
1 way to stay at step 0
1 way to reach step 1
```

---

# Memoization

```cpp id="jlwm66"
solve(n) =
solve(n-1) + solve(n-2)
```

Store computed states.

---

# Tabulation

```cpp id="jlwm77"
for(int i = 2; i <= n; i++){

    dp[i] = dp[i-1] + dp[i-2];
}
```

---

# Space Optimization

Observation:

```txt id="jlwm88"
Only dp[i-1] and dp[i-2]
are needed.
```

Use:

```cpp id="jlwm99"
prev2 = 1;
prev1 = 1;
```

```cpp id="jlwm00"
curr = prev1 + prev2;
```

Update:

```cpp id="jlwm12"
prev2 = prev1;
prev1 = curr;
```

---

# Optimal Code Pattern

```cpp id="jlwm13"
int prev2 = 1;
int prev1 = 1;

for(int i = 2; i <= n; i++){

    int curr = prev1 + prev2;

    prev2 = prev1;
    prev1 = curr;
}

return prev1;
```

---

# Complexity

### Tabulation

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n)       |
| Space  | O(n)       |

---

### Space Optimized

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n)       |
| Space  | O(1)       |

---

# Pattern Recognition

Whenever:

```txt id="jlwm14"
Current answer depends on
previous few states
```

Think:

```txt id="jlwm15"
DP
```

---

# DP Workflow

```txt id="jlwm16"
State
→ Recurrence
→ Base Cases
→ Memoization
→ Tabulation
→ Space Optimization
```

---

# Important Insight

This problem is essentially:

F(n)=F(n-1)+F(n-2)

So Climbing Stairs is just the Fibonacci sequence in disguise.

---

# Takeaway

```txt id="jlwm17"
The first DP problem teaches:

1. Define state
2. Write recurrence
3. Convert recursion to DP
4. Optimize space when only previous states are needed
```
