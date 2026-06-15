# Palindrome Partitioning II (LeetCode 132)

## Problem Statement

Given a string `s`, partition `s` such that every substring of the partition is a palindrome.

Return the **minimum cuts** needed to partition the string into palindrome substrings.

### Example

```text
Input: s = "aab"
Output: 1
Explanation:
aa | b
```

---

# Key Idea

Whenever we make a cut, both resulting parts must eventually become palindrome partitions.

The goal is to minimize the number of cuts.

---

# Approach 1: Pure Recursion (MCM Style)

## Thinking

For every partition point `k`:

```text
s[i...k] | s[k+1...j]
```

Find:

```text
cuts(left) + cuts(right) + 1
```

Take minimum over all possible `k`.

## Recurrence

```text
f(i,j) =
min(
    f(i,k) + f(k+1,j) + 1
)
```

## Complexity

```text
Time  : Exponential
Space : O(n)
```

---

# Approach 2: Memoization (MCM + DP)

## State

```text
dp[i][j]
=
minimum cuts required for substring s[i...j]
```

## Complexity

```text
States      = O(n²)
Transition  = O(n)
Palindrome Check = O(n)

Total = O(n⁴)
```

## Why Still Slow?

For every state:

1. Try every partition.
2. Check palindrome repeatedly.
3. Solve both left and right sides.

---

# Approach 3: Optimized Memoization (Palindrome Prefix Strategy)

## Key Observation

If:

```text
s[i...k]
```

is already a palindrome, then:

```text
minCuts(s[i...k]) = 0
```

No need to recursively solve the left side.

Only solve the remaining suffix.

## New Recurrence

```text
answer =
1 + solve(k+1, j)
```

where `s[i...k]` is a palindrome.

## Complexity

```text
States = O(n²)
Transitions = O(n)
Palindrome Check = O(n)

Total = O(n³)
```

---

# Interview Revision

## Pure Recursion

- Try every partition.
- Solve left + right.
- Exponential.

## Memoization

- Store dp[i][j].
- Avoid repeated states.
- O(n⁴).

## Optimized Memoization

- Cut only after a palindrome prefix.
- Skip solving left side.
- O(n³).

## Golden Insight

```text
If the left part is already a palindrome,
its minimum cut is always 0.
```

Therefore:

```text
Don't recursively solve the left side.
Only solve the remaining suffix.
```
