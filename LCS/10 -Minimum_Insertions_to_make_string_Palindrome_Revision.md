# Minimum Insertions to Make a String Palindrome

## Key Idea

Minimum insertions required to make a string a palindrome:

```text
Min Insertions = n - LPS
```

Where:

```text
LPS = Longest Palindromic Subsequence
```

LPS can be found using:

```text
LPS(s) = LCS(s, reverse(s))
```

Therefore:

```text
Answer = n - LCS(s, reverse(s))
```

---

# 1. Recursive Approach (Pure Recursion)

```java
class Solution {

    public int minInsertions(String s) {
        String rev = new StringBuilder(s).reverse().toString();

        int lps = lcs(s, rev, s.length(), rev.length());

        return s.length() - lps;
    }

    private int lcs(String s1, String s2, int n, int m) {

        if (n == 0 || m == 0)
            return 0;

        if (s1.charAt(n - 1) == s2.charAt(m - 1))
            return 1 + lcs(s1, s2, n - 1, m - 1);

        return Math.max(
                lcs(s1, s2, n - 1, m),
                lcs(s1, s2, n, m - 1)
        );
    }
}
```

### Complexity

- Time: O(2^(n+m))
- Space: O(n+m)

---

# 2. Top Down DP (Memoization)

```java
class Solution {

    public int minInsertions(String s) {

        String rev = new StringBuilder(s).reverse().toString();

        int n = s.length();

        int[][] dp = new int[n + 1][n + 1];

        for (int[] row : dp)
            java.util.Arrays.fill(row, -1);

        int lps = lcs(s, rev, n, n, dp);

        return n - lps;
    }

    private int lcs(String s1, String s2, int n, int m, int[][] dp) {

        if (n == 0 || m == 0)
            return 0;

        if (dp[n][m] != -1)
            return dp[n][m];

        if (s1.charAt(n - 1) == s2.charAt(m - 1)) {
            return dp[n][m] =
                    1 + lcs(s1, s2, n - 1, m - 1, dp);
        }

        return dp[n][m] = Math.max(
                lcs(s1, s2, n - 1, m, dp),
                lcs(s1, s2, n, m - 1, dp)
        );
    }
}
```

### Complexity

- Time: O(n²)
- Space: O(n²)

---

# 3. Bottom Up DP (Tabulation)

```java
class Solution {

    public int minInsertions(String s) {

        int n = s.length();

        String rev = new StringBuilder(s).reverse().toString();

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

# Revision Notes

### Pattern Identification

When you see:

- Minimum insertions to make palindrome
- Minimum deletions to make palindrome
- Longest Palindromic Subsequence

Think:

```text
String + Reverse(String)
→ Apply LCS
```

---

### Formula Sheet

```text
LPS = LCS(s, reverse(s))

Min Insertions = n - LPS

Min Deletions = n - LPS
```

---

### Example

```text
s = "mbadm"

reverse = "mdabm"

LCS = 3 ("mam")

Min Insertions
= 5 - 3
= 2
```

---

### Interview Explanation (30 Seconds)

1. A palindrome reads the same forward and backward.
2. Find the Longest Palindromic Subsequence.
3. LPS can be obtained by finding LCS between the string and its reverse.
4. Characters not part of LPS must be inserted.
5. Hence answer = n − LPS.

---

# DP Pattern Mapping

```text
Palindrome Problems
        |
        v
Longest Palindromic Subsequence
        |
        v
LCS(s, reverse(s))
        |
        v
Answer = n - LPS
```

This is one of the most important LCS variations for interviews.
