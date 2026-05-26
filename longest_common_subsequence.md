# Longest Common Subsequence (LCS) - Complete Revision Notes

# Problem Statement

Given two strings `text1` and `text2`, return the length of their Longest Common Subsequence.

A subsequence is a sequence that appears in the same relative order but not necessarily contiguous.

---

# Core Idea

At every position:

- If characters match → include them in answer.
- If characters do not match → try skipping one character from either string.

---

# 1. Recursive Solution (Brute Force)

## Intuition

We compare characters from the end of both strings.

### Case 1: Characters Match

```java
if(text1.charAt(i) == text2.charAt(j))
```

then:

```java
1 + helper(i-1, j-1)
```

### Case 2: Characters Do Not Match

Try both possibilities:

- Skip from first string
- Skip from second string

Take maximum.

---

## Recursive Code

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int n = text1.length();
        int m = text2.length();

        return helper(text1, text2, n - 1, m - 1);
    }

    public int helper(String text1, String text2, int i, int j) {

        if(i < 0 || j < 0) {
            return 0;
        }

        if(text1.charAt(i) == text2.charAt(j)) {
            return 1 + helper(text1, text2, i - 1, j - 1);
        }

        return Math.max(
            helper(text1, text2, i - 1, j),
            helper(text1, text2, i, j - 1)
        );
    }
}
```

---

## Recursive Tree Idea

```text
helper(i, j)
├── helper(i-1, j)
└── helper(i, j-1)
```

---

## Choice Diagram

```text
if match:
    1 + helper(i-1, j-1)

else:
    max(helper(i-1, j), helper(i, j-1))
```

---

## Complexity

Time Complexity:

```text
O(2^(n+m))
```

Space Complexity:

```text
O(n+m)
```

---

# 2. Memoization (Top Down DP)

## Key Idea

Store already solved states in DP table.

State:

```text
(i, j)
```

---

## Memoization Code

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int n=text1.length();
        int m=text2.length();

        Integer[][] dp= new Integer[n+1][m+1];

        return helper(text1, text2, n-1 , m-1, dp);
    }

    public int helper(String text1, String text2, int i , int j, Integer[][] dp){

        if(i<0 || j<0){
            return 0;
        }

        if(dp[i][j] != null){
            return dp[i][j];
        }

        if(text1.charAt(i) == text2.charAt(j)){
            dp[i][j]= 1+ helper(text1, text2, i-1, j-1, dp);
        }else{
            dp[i][j]= Math.max(
                helper(text1, text2, i-1, j, dp),
                helper(text1, text2, i, j-1, dp)
            );
        }

        return dp[i][j];
    }
}
```

---

## Complexity

Time Complexity:

```text
O(n*m)
```

Space Complexity:

```text
O(n*m) + O(n+m)
```

---

# 3. Bottom Up DP (Tabulation)

## DP Meaning

```text
dp[i][j]
```

means:

```text
LCS of first i chars of text1
and first j chars of text2
```

---

## Bottom Up Code

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {

        int n = text1.length();
        int m = text2.length();

        int[][] dp = new int[n + 1][m + 1];

        for(int i = 1; i <= n; i++) {

            for(int j = 1; j <= m; j++) {

                if(text1.charAt(i - 1) == text2.charAt(j - 1)) {

                    dp[i][j] = 1 + dp[i - 1][j - 1];

                } else {

                    dp[i][j] = Math.max(
                        dp[i - 1][j],
                        dp[i][j - 1]
                    );
                }
            }
        }

        return dp[n][m];
    }
}
```

---

## Complexity

Time Complexity:

```text
O(n*m)
```

Space Complexity:

```text
O(n*m)
```

---

# 4. Space Optimized DP

## Key Observation

Current row depends only on:

- previous row
- current row left value

---

## Space Optimized Code

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {

        int n = text1.length();
        int m = text2.length();

        int[] prev = new int[m + 1];

        for(int i = 1; i <= n; i++) {

            int[] curr = new int[m + 1];

            for(int j = 1; j <= m; j++) {

                if(text1.charAt(i - 1) == text2.charAt(j - 1)) {

                    curr[j] = 1 + prev[j - 1];

                } else {

                    curr[j] = Math.max(prev[j], curr[j - 1]);
                }
            }

            prev = curr;
        }

        return prev[m];
    }
}
```

---

## Complexity

Time Complexity:

```text
O(n*m)
```

Space Complexity:

```text
O(m)
```

---

# Quick Revision Table

| Approach | Time | Space |
|---|---|---|
| Recursion | O(2^(n+m)) | O(n+m) |
| Memoization | O(n*m) | O(n*m) |
| Tabulation | O(n*m) | O(n*m) |
| Space Optimized | O(n*m) | O(m) |

---

# Important Concepts

## Why use n+1 and m+1?

To represent empty string states and avoid negative indices.

## Why charAt(i-1)?

Because dp[i][j] means first i characters.
