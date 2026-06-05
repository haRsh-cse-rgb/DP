# Longest Repeating Subsequence (LRS)

## Intuition

LRS is similar to LCS. We find the LCS of the string with itself, but we do not allow matching the same index.

Condition:

s[i-1] == s[j-1] AND i != j

---

## Key Idea

LRS = LCS(s, s)

with the extra constraint:

i != j

---

## 1. Recursive Solution

```java
class Solution {

    public int LongestRepeatingSubsequence(String s) {
        return helper(s, 0, 0);
    }

    private int helper(String s, int i, int j) {

        if (i == s.length() || j == s.length()) {
            return 0;
        }

        if (s.charAt(i) == s.charAt(j) && i != j) {
            return 1 + helper(s, i + 1, j + 1);
        }

        return Math.max(
            helper(s, i + 1, j),
            helper(s, i, j + 1)
        );
    }
}
```

### Complexity

- Time: Exponential
- Space: O(n)

---

## 2. Memoization (Top Down)

```java
class Solution {

    public int LongestRepeatingSubsequence(String s) {

        int n = s.length();
        Integer[][] dp = new Integer[n][n];

        return helper(s, 0, 0, dp);
    }

    private int helper(String s, int i, int j, Integer[][] dp) {

        if (i == s.length() || j == s.length()) {
            return 0;
        }

        if (dp[i][j] != null) {
            return dp[i][j];
        }

        if (s.charAt(i) == s.charAt(j) && i != j) {
            return dp[i][j] =
                1 + helper(s, i + 1, j + 1, dp);
        }

        return dp[i][j] = Math.max(
            helper(s, i + 1, j, dp),
            helper(s, i, j + 1, dp)
        );
    }
}
```

### Complexity

- Time: O(n²)
- Space: O(n²)

---

## 3. Bottom Up DP (Your Code)

```java
class Solution {
    public int LongestRepeatingSubsequence(String s) {

        int n = s.length();

        int[][] dp = new int[n + 1][n + 1];

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {

                if (s.charAt(i - 1) == s.charAt(j - 1)
                        && i != j) {

                    dp[i][j] = 1 + dp[i - 1][j - 1];

                } else {

                    dp[i][j] = Math.max(
                        dp[i - 1][j],
                        dp[i][j - 1]
                    );
                }
            }
        }

        return dp[n][n];
    }
}
```

### Complexity

- Time: O(n²)
- Space: O(n²)

---

## Printing LRS

Same as LCS backtracking.

Take diagonal only when:

```text
s[i-1] == s[j-1] && i != j
```

---

## Quick Revision

```text
LRS = LCS(s, s)

Match:
s[i-1] == s[j-1] && i != j

If match:
1 + dp[i-1][j-1]

Else:
max(dp[i-1][j], dp[i][j-1])
```

Interview One-Liner:

Longest Repeating Subsequence is LCS of a string with itself while ensuring the same index is never matched.
