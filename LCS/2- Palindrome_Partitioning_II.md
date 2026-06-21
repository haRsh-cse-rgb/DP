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

---

## Recurrence

```text
f(i,j) =
min(
    f(i,k) + f(k+1,j) + 1
)
```

---

## Base Cases

```java
if(i >= j)
    return 0;

if(isPalindrome(i,j))
    return 0;
```

---

## Code

```java
class Solution {

    public int minCut(String s) {
        return solve(s, 0, s.length()-1);
    }

    int solve(String s, int i, int j){

        if(i >= j)
            return 0;

        if(isPalindrome(s, i, j))
            return 0;

        int ans = Integer.MAX_VALUE;

        for(int k=i; k<j; k++){

            int temp =
                    solve(s, i, k)
                  + solve(s, k+1, j)
                  + 1;

            ans = Math.min(ans, temp);
        }

        return ans;
    }

    boolean isPalindrome(String s, int i, int j){

        while(i < j){

            if(s.charAt(i) != s.charAt(j))
                return false;

            i++;
            j--;
        }

        return true;
    }
}
```

---

## Complexity

```text
Time  : Exponential
Space : O(n)
```

### Problem

Same subproblems are solved repeatedly.

Leads to TLE.

---

# Approach 2: Memoization (MCM + DP)

## Thinking

Store answers for every state:

```text
dp[i][j]
```

so that repeated calculations are avoided.

---

## State

```text
dp[i][j]
=
minimum cuts required for substring s[i...j]
```

---

## Code

```java
class Solution {

    public int minCut(String s) {

        int n = s.length();

        Integer[][] dp = new Integer[n][n];

        return solve(s, 0, n-1, dp);
    }

    int solve(String s, int i, int j, Integer[][] dp){

        if(i >= j)
            return 0;

        if(isPalindrome(s, i, j))
            return 0;

        if(dp[i][j] != null)
            return dp[i][j];

        int ans = Integer.MAX_VALUE;

        for(int k=i; k<j; k++){

            int temp =
                    solve(s, i, k, dp)
                  + solve(s, k+1, j, dp)
                  + 1;

            ans = Math.min(ans, temp);
        }

        return dp[i][j] = ans;
    }

    boolean isPalindrome(String s, int i, int j){

        while(i < j){

            if(s.charAt(i) != s.charAt(j))
                return false;

            i++;
            j--;
        }

        return true;
    }
}
```

---

## Complexity

```text
States      = O(n²)

Transition  = O(n)

Palindrome Check = O(n)
```

```text
Total = O(n⁴)
```

---

## Why Still Slow?

For every state:

```text
(i,j)
```

we:

1. Try every k.
2. Check palindrome repeatedly.
3. Solve both left and right sides.

Still causes TLE for larger inputs.

---

# Approach 3: Optimized Memoization (Palindrome Prefix Strategy)

## Key Observation

Instead of partitioning both sides:

```text
left | right
```

only make a cut if:

```text
left is already a palindrome
```

Because:

```text
minCuts(palindrome) = 0
```

No need to recursively solve the left side.

---

## New Recurrence

If:

```text
s[i...k]
```

is palindrome:

```text
answer =
1 + solve(k+1, j)
```

Take minimum over all valid palindrome prefixes.

---

## Code

```java
class Solution {

    public int minCut(String s) {

        int n = s.length();

        Integer[][] dp = new Integer[n][n];

        return solve(s, 0, n-1, dp);
    }

    int solve(String s, int i, int j, Integer[][] dp){

        if(i >= j)
            return 0;

        if(isPalindrome(s, i, j))
            return 0;

        if(dp[i][j] != null)
            return dp[i][j];

        int ans = Integer.MAX_VALUE;

        for(int k=i; k<j; k++){

            if(isPalindrome(s, i, k)){

                ans = Math.min(
                        ans,
                        1 + solve(s, k+1, j, dp)
                );
            }
        }

        return dp[i][j] = ans;
    }

    boolean isPalindrome(String s, int i, int j){

        while(i < j){

            if(s.charAt(i) != s.charAt(j))
                return false;

            i++;
            j--;
        }

        return true;
    }
}
```

---

## Why It Works

Suppose:

```text
aa | bccb
```

The left part:

```text
aa
```

is already a palindrome.

Therefore:

```text
cuts(aa) = 0
```

No need to calculate it recursively.

Only solve:

```text
bccb
```

This removes half of the recursion work.

---

## Complexity

Without palindrome precomputation:

```text
States = O(n²)

Transitions = O(n)

Palindrome Check = O(n)
```

```text
Total = O(n³)
```

Much faster than the previous memoized version.

---

# Final Interview Revision

## Approach 1

```text
Pure Recursion
```

Idea:

```text
Try every partition
Solve left + right
```

Complexity:

```text
Exponential
```

---

## Approach 2

```text
Memoization
```

Idea:

```text
Store dp[i][j]
```

Complexity:

```text
O(n⁴)
```

Reason:

```text
O(n²) states
× O(n) partitions
× O(n) palindrome check
```

---

## Approach 3

```text
Optimized Memoization
```

Idea:

```text
Only cut after a palindrome prefix
```

Recurrence:

```text
f(i,j) =
min(
    1 + f(k+1,j)
)
```

where:

```text
s[i...k] is palindrome
```

Complexity:

```text
O(n³)
```

---

# Most Important Takeaway

The optimization comes from realizing:

```text
If left part is already a palindrome,
its minimum cut is always 0.
```

Therefore:

```text
Don't recursively solve the left side.
Only solve the remaining suffix.
```

This transforms the classic MCM recurrence into a much faster palindrome-prefix recurrence.
