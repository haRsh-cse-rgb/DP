# Longest Common Subsequence (LCS) — Complete Notes

# Problem

Given two strings:

```java
text1 = "abcde"
text2 = "ace"
```

Find:

1. Length of the Longest Common Subsequence
2. Actual subsequence string

Output:

```java
ace
```

---

# Core Idea of LCS

A subsequence means:

- Characters remain in same order
- Characters may be skipped

Example:

```text
abcde
```

Possible subsequences:

```text
a
ab
ace
bde
```

Not valid:

```text
aec
```

because order changed.

---

# Main DP Thinking

At every position `(i, j)`:

We compare:

```java
text1.charAt(i - 1)
text2.charAt(j - 1)
```

Two cases:

---

## CASE 1 — Characters Match

If:

```java
text1[i - 1] == text2[j - 1]
```

then:

```java
dp[i][j] = 1 + dp[i - 1][j - 1]
```

Why?

Because current character becomes part of LCS.

Diagram:

```text
a b c d e
        ↑

a c e
    ↑
```

Both are `e`.

So:

```text
previous answer + 1
```

---

## CASE 2 — Characters Do NOT Match

Then we try both possibilities:

1. Ignore character from text1
2. Ignore character from text2

Formula:

```java
dp[i][j] = Math.max(
    dp[i - 1][j],
    dp[i][j - 1]
)
```

---

# Bottom-Up DP Solution

```java
class Solution {

    public String longestCommonSubsequence(String text1, String text2) {

        int n = text1.length();
        int m = text2.length();

        int[][] dp = new int[n + 1][m + 1];

        // Build DP table
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {

                // Characters matched
                if (text1.charAt(i - 1) == text2.charAt(j - 1)) {

                    dp[i][j] = 1 + dp[i - 1][j - 1];

                } else {

                    // Take maximum of left and top
                    dp[i][j] = Math.max(
                        dp[i - 1][j],
                        dp[i][j - 1]
                    );
                }
            }
        }

        // Build actual subsequence
        StringBuilder ans = new StringBuilder();

        int i = n;
        int j = m;

        while (i > 0 && j > 0) {

            // Character matched
            if (text1.charAt(i - 1) == text2.charAt(j - 1)) {

                ans.append(text1.charAt(i - 1));

                i--;
                j--;
            }

            // Move toward larger value
            else if (dp[i - 1][j] > dp[i][j - 1]) {

                i--;

            } else {

                j--;
            }
        }

        // Reverse because answer built backwards
        return ans.reverse().toString();
    }
}
```

---

# Understanding the DP Table

Example:

```java
text1 = "abcde"
text2 = "ace"
```

DP Table:

|     | "" | a | c | e |
|-----|----|---|---|---|
| ""  | 0  | 0 | 0 | 0 |
| a   | 0  | 1 | 1 | 1 |
| b   | 0  | 1 | 1 | 1 |
| c   | 0  | 1 | 2 | 2 |
| d   | 0  | 1 | 2 | 2 |
| e   | 0  | 1 | 2 | 3 |

---

# How Table Was Filled

---

## Step 1

Comparing:

```text
a vs a
```

Match.

So:

```java
dp[1][1] = 1 + dp[0][0]
         = 1
```

---

## Step 2

Comparing:

```text
b vs a
```

No match.

So:

```java
dp[2][1] = max(
    dp[1][1],
    dp[2][0]
)
= max(1, 0)
= 1
```

---

## Step 3

Comparing:

```text
c vs c
```

Match.

```java
dp[3][2] = 1 + dp[2][1]
         = 2
```

---

## Step 4

Comparing:

```text
e vs e
```

Match.

```java
dp[5][3] = 1 + dp[4][2]
         = 3
```

Final LCS length:

```text
3
```

Final LCS string:

```text
ace
```

---

# Why Backtracking Works

We start from:

```java
dp[n][m]
```

If characters matched:

```java
text1.charAt(i - 1) == text2.charAt(j - 1)
```

then that character definitely belongs to LCS.

So:

```java
ans.append(character)
i--
j--
```

---

If NOT matched:

Move toward larger DP value.

```java
if(dp[i - 1][j] > dp[i][j - 1])
```

move upward.

Else:

move left.

This traces the path that created the optimal answer.

---

# Why reverse() is Needed

During backtracking:

We start from end.

Example:

```text
e
c
a
```

So StringBuilder becomes:

```text
eca
```

Reverse converts it to:

```text
ace
```

---

# Time Complexity (Bottom-Up)

## DP Table Filling

Rows:

```text
n
```

Columns:

```text
m
```

Each cell computed once.

So:

```text
O(n * m)
```

---

## Backtracking

At most:

```text
n + m
```

steps.

So:

```text
O(n + m)
```

---

## Final Complexity

Time:

```text
O(n * m)
```

Space:

```text
O(n * m)
```

---

# Recursive + Memoization Version

This version keeps recursive thinking.

---

```java
class Solution {

    int[][] dp;

    public String longestCommonSubsequence(String text1, String text2) {

        int n = text1.length();
        int m = text2.length();

        dp = new int[n][m];

        // Fill with -1
        for (int i = 0; i < n; i++) {
            Arrays.fill(dp[i], -1);
        }

        // Fill DP recursively
        helper(text1, text2, n - 1, m - 1);

        // Build actual answer
        return build(text1, text2, n - 1, m - 1);
    }

    // Returns LCS length
    public int helper(String text1, String text2, int i, int j) {

        // Base case
        if (i < 0 || j < 0) {
            return 0;
        }

        // Already computed
        if (dp[i][j] != -1) {
            return dp[i][j];
        }

        // Characters matched
        if (text1.charAt(i) == text2.charAt(j)) {

            dp[i][j] =
                1 + helper(text1, text2, i - 1, j - 1);

        } else {

            int left =
                helper(text1, text2, i - 1, j);

            int right =
                helper(text1, text2, i, j - 1);

            dp[i][j] = Math.max(left, right);
        }

        return dp[i][j];
    }

    // Reconstruct answer recursively
    public String build(String text1, String text2, int i, int j) {

        if (i < 0 || j < 0) {
            return "";
        }

        // Character matched
        if (text1.charAt(i) == text2.charAt(j)) {

            return build(
                text1,
                text2,
                i - 1,
                j - 1
            ) + text1.charAt(i);
        }

        // Move toward larger DP value
        if (i > 0) {

            if (j > 0) {

                if (dp[i - 1][j] >= dp[i][j - 1]) {

                    return build(
                        text1,
                        text2,
                        i - 1,
                        j
                    );

                } else {

                    return build(
                        text1,
                        text2,
                        i,
                        j - 1
                    );
                }

            } else {

                return build(
                    text1,
                    text2,
                    i - 1,
                    j
                );
            }

        } else {

            return build(
                text1,
                text2,
                i,
                j - 1
            );
        }
    }
}
```

---

# Recursive Memoization Flow

Suppose:

```java
helper(4, 2)
```

Comparing:

```text
e vs e
```

Match.

So:

```java
1 + helper(3, 1)
```

Then:

```java
helper(3,1)
```

Comparing:

```text
d vs c
```

No match.

So:

```java
max(
    helper(2,1),
    helper(3,0)
)
```

Memoization ensures:

Each `(i,j)` computed only once.

---

# Complexity of Recursive + Memoization

## Time

Each state:

```text
(i,j)
```

computed once.

Total states:

```text
n * m
```

So:

```text
O(n * m)
```

---

## Space

DP array:

```text
O(n * m)
```

Recursion stack:

```text
O(n + m)
```

Total:

```text
O(n * m)
```

---

# Important Memory Points

---

## Bottom-Up

- Easier to optimize space
- Iterative
- Faster in interviews usually

---

## Recursive + Memoization

- Easier to think initially
- Mirrors recurrence naturally
- Good for understanding DP transitions

---

# Most Important Intuition

LCS always asks:

```text
Current characters useful or not?
```

If useful:

```text
Take both
```

Else:

```text
Try skipping one side
```

That single idea generates the whole algorithm.
