# Shortest Common Supersequence (SCS) Length

## Key Relationship with LCS

The shortest common supersequence length can be calculated using:

SCS Length = len(s1) + len(s2) - LCS Length

Why?

- LCS characters are common in both strings.
- If we simply concatenate both strings, the common characters get counted twice.
- Subtracting the LCS length removes the duplicate count.

Example:

s1 = "abac"
s2 = "cab"

LCS = "ab" (length = 2)

SCS Length = 4 + 3 - 2 = 5

---

# 1. Pure Recursion (LCS)

## Idea

At every position:

- If characters match:
  - Include that character in LCS.
  - Move both pointers.
- Otherwise:
  - Try skipping a character from either string.
  - Take the maximum result.

## Time Complexity

O(2^(n+m))

## Space Complexity

O(n+m) recursion stack

```java
class Solution {

    public static int minSuperSeq(String s1, String s2) {
        int lcs = helper(s1, s2, 0, 0);
        return s1.length() + s2.length() - lcs;
    }

    static int helper(String s1, String s2, int i, int j) {

        if (i == s1.length() || j == s2.length()) {
            return 0;
        }

        if (s1.charAt(i) == s2.charAt(j)) {
            return 1 + helper(s1, s2, i + 1, j + 1);
        }

        return Math.max(
            helper(s1, s2, i + 1, j),
            helper(s1, s2, i, j + 1)
        );
    }
}
```

---

# 2. Memoization (Top-Down DP)

## Idea

The recursion revisits the same states many times.

State:

dp[i][j] = LCS length starting from indices i and j

Store computed answers and reuse them.

## Time Complexity

O(n × m)

## Space Complexity

O(n × m) + recursion stack

```java
class Solution {

    public static int minSuperSeq(String s1, String s2) {

        int n = s1.length();
        int m = s2.length();

        Integer[][] dp = new Integer[n][m];

        int lcs = helper(s1, s2, 0, 0, dp);

        return n + m - lcs;
    }

    static int helper(
        String s1,
        String s2,
        int i,
        int j,
        Integer[][] dp
    ) {

        if (i == s1.length() || j == s2.length()) {
            return 0;
        }

        if (dp[i][j] != null) {
            return dp[i][j];
        }

        if (s1.charAt(i) == s2.charAt(j)) {
            return dp[i][j] =
                1 + helper(s1, s2, i + 1, j + 1, dp);
        }

        return dp[i][j] = Math.max(
            helper(s1, s2, i + 1, j, dp),
            helper(s1, s2, i, j + 1, dp)
        );
    }
}
```

---

# Common Bug in Memoization

Wrong:

```java
if(s1.charAt(i) == s2.charAt(j)){
    dp[i][j] = 1 + helper(...);
}

dp[i][j] = Math.max(...);
```

What happens?

If characters match:

```java
dp[i][j] = 5;
```

Then immediately:

```java
dp[i][j] = 3;
```

The first answer gets overwritten.

Correct:

```java
if(match){
    return dp[i][j] = 1 + helper(...);
}

return dp[i][j] = Math.max(...);
```

or

```java
if(match){
    dp[i][j] = 1 + helper(...);
}
else{
    dp[i][j] = Math.max(...);
}
```

---

# 3. Bottom-Up DP

## Idea

Build LCS table from smaller subproblems.

Definition:

dp[i][j] = LCS length of

s1[0...i-1]
s2[0...j-1]

Transition:

If characters match:

dp[i][j] = 1 + dp[i-1][j-1]

Otherwise:

dp[i][j] =
max(dp[i-1][j], dp[i][j-1])

## Time Complexity

O(n × m)

## Space Complexity

O(n × m)

```java
class Solution {

    public static int minSuperSeq(String s1, String s2) {

        int n = s1.length();
        int m = s2.length();

        int[][] dp = new int[n + 1][m + 1];

        for (int i = 1; i <= n; i++) {

            for (int j = 1; j <= m; j++) {

                if (s1.charAt(i - 1) == s2.charAt(j - 1)) {

                    dp[i][j] = 1 + dp[i - 1][j - 1];

                } else {

                    dp[i][j] = Math.max(
                        dp[i - 1][j],
                        dp[i][j - 1]
                    );
                }
            }
        }

        int lcs = dp[n][m];

        return n + m - lcs;
    }
}
```

---

# Pattern Recognition

Whenever you see:

- Shortest Common Supersequence
- Minimum insertions/deletions between strings
- Longest Palindromic Subsequence
- Minimum deletions to make palindrome

Think:

"Can this be converted into an LCS problem?"

Many string DP problems are simply LCS in disguise.
