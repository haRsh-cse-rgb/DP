# Minimum Deletions to Make a String Palindrome

## Key Idea

Minimum deletions required =

`n - Longest Palindromic Subsequence (LPS)`

Since:

`LPS = LCS(s, reverse(s))`

---

# 1. Recursive Solution

```java
class Solution {
    public int minDeletions(String s) {
        String rev = new StringBuilder(s).reverse().toString();
        int lps = helper(s, rev, 0, 0);
        return s.length() - lps;
    }

    private int helper(String s, String rev, int i, int j) {
        if (i == s.length() || j == rev.length()) {
            return 0;
        }

        if (s.charAt(i) == rev.charAt(j)) {
            return 1 + helper(s, rev, i + 1, j + 1);
        }

        return Math.max(
            helper(s, rev, i + 1, j),
            helper(s, rev, i, j + 1)
        );
    }
}
```

### Complexity

- Time: O(2^(n+m))
- Space: O(n+m)

---

# 2. Memoization (Top-Down DP)

```java
class Solution {
    public int minDeletions(String s) {
        String rev = new StringBuilder(s).reverse().toString();
        int n = s.length();

        Integer[][] dp = new Integer[n + 1][n + 1];

        int lps = helper(s, rev, 0, 0, dp);

        return n - lps;
    }

    private int helper(String s, String rev, int i, int j, Integer[][] dp) {
        if (i == s.length() || j == rev.length()) {
            return 0;
        }

        if (dp[i][j] != null) {
            return dp[i][j];
        }

        if (s.charAt(i) == rev.charAt(j)) {
            return dp[i][j] =
                1 + helper(s, rev, i + 1, j + 1, dp);
        }

        return dp[i][j] = Math.max(
            helper(s, rev, i + 1, j, dp),
            helper(s, rev, i, j + 1, dp)
        );
    }
}
```

### Complexity

- Time: O(n²)
- Space: O(n²)

---

# 3. Bottom-Up DP (Tabulation)

```java
class Solution {
    public int minDeletions(String s) {
        String rev = new StringBuilder(s).reverse().toString();

        int n = s.length();

        int[][] dp = new int[n + 1][n + 1];

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {

                if (s.charAt(i - 1) == rev.charAt(j - 1)) {
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                } else {
                    dp[i][j] = Math.max(
                        dp[i - 1][j],
                        dp[i][j - 1]
                    );
                }
            }
        }

        int lps = dp[n][n];

        return n - lps;
    }
}
```

### Complexity

- Time: O(n²)
- Space: O(n²)

---

# Quick Revision

1. Reverse the string.
2. Find LCS between original string and reversed string.
3. LCS length = Longest Palindromic Subsequence.
4. Answer = `n - LPS`.

### Formula

```text
Minimum Deletions = Length of String - LPS
LPS = LCS(s, reverse(s))
```
