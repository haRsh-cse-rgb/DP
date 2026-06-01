# LeetCode 583 - Delete Operation for Two Strings

## Problem Statement

Given two strings `word1` and `word2`, return the minimum number of steps required to make the two strings the same.

In one step, you can delete exactly one character from either string.

### Example

```text
Input:
word1 = "sea"
word2 = "eat"

Output:
2

Explanation:
Delete 's' from "sea" -> "ea"
Delete 't' from "eat" -> "ea"
```

---

# Key Intuition

The goal is to make both strings identical.

Instead of directly thinking about deletions, think about:

> What is the longest sequence of characters we can keep in both strings?

That sequence is the **Longest Common Subsequence (LCS)**.

If:

```text
LCS Length = L
```

Then:

```text
Characters to delete from word1 = n - L
Characters to delete from word2 = m - L
```

Therefore:

```text
Minimum Deletions
= (n - L) + (m - L)
= n + m - 2L
```

---

# Triangle Visualization

Example:

```text
word1 = sea
word2 = eat

LCS = "ea" (length = 2)
```

```text
                 LCS
                  ea
                /    \
               /      \
      word1 = sea    word2 = eat
          |              |
    delete 's'      delete 't'
          |              |
          ea            ea
```

Calculation:

```text
n = 3
m = 3
LCS = 2

Delete from word1:
3 - 2 = 1

Delete from word2:
3 - 2 = 1

Total = 1 + 1 = 2
```

---

# Recursive Solution

## Recurrence

Let:

```text
f(i, j)
= LCS length of first i characters of word1
  and first j characters of word2
```

If characters match:

```text
f(i,j) = 1 + f(i-1,j-1)
```

Else:

```text
f(i,j) =
max(
    f(i-1,j),
    f(i,j-1)
)
```

### Recursive Code

```java
class Solution {

    int lcs(String s1, String s2, int i, int j) {

        if (i == 0 || j == 0) {
            return 0;
        }

        if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
            return 1 + lcs(s1, s2, i - 1, j - 1);
        }

        return Math.max(
                lcs(s1, s2, i - 1, j),
                lcs(s1, s2, i, j - 1)
        );
    }

    public int minDistance(String word1, String word2) {

        int lcsLength = lcs(
                word1,
                word2,
                word1.length(),
                word2.length()
        );

        return (word1.length() - lcsLength)
                + (word2.length() - lcsLength);
    }
}
```

### Complexity

```text
Time  : O(2^(n+m))
Space : O(n+m)
```

---

# DP Solution (LCS)

```java
class Solution {
    public int minDistance(String word1, String word2) {

        int n = word1.length();
        int m = word2.length();

        int[][] dp = new int[n + 1][m + 1];

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {

                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                } else {
                    dp[i][j] = Math.max(
                            dp[i - 1][j],
                            dp[i][j - 1]
                    );
                }
            }
        }

        int lcsLength = dp[n][m];

        return (n - lcsLength)
                + (m - lcsLength);
    }
}
```

---

# DP Table Idea

```text
If characters match:
↖ diagonal + 1

If characters don't match:
max(↑, ←)
```

```text
      e  a  t
   0  0  0  0
s  0  0  0  0
e  0  1  1  1
a  0  1  2  2
```

LCS Length = 2

---

# Complexity Analysis

## Recursive

```text
Time  : O(2^(n+m))
Space : O(n+m)
```

## DP (Tabulation)

```text
Time  : O(n × m)
Space : O(n × m)
```

---

# Final Formula

```text
Minimum Deletions
= (Length of Word1 - LCS)
+ (Length of Word2 - LCS)

= n + m - 2 × LCS
```

This problem is essentially an LCS problem in disguise.
