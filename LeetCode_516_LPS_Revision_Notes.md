# Longest Palindromic Subsequence (LeetCode 516)

## Quick Revision

- Reverse the string.
- Find LCS between original string and reversed string.
- LPS = LCS(s, reverse(s))

### Pattern

Palindrome → Reverse String → LCS

---

## Approach 1: Recursion

### Recurrence

If characters match:

f(i,j) = 1 + f(i-1,j-1)

Else:

f(i,j) = max(f(i-1,j), f(i,j-1))

### Base Case

i == 0 or j == 0 → 0

### Complexity

Time: Exponential

Space: O(n)

```java
class Solution {

    int lcs(String s, String rev, int i, int j){

        if(i == 0 || j == 0) return 0;

        if(s.charAt(i-1) == rev.charAt(j-1))
            return 1 + lcs(s, rev, i-1, j-1);

        return Math.max(
            lcs(s, rev, i-1, j),
            lcs(s, rev, i, j-1)
        );
    }

    public int longestPalindromeSubseq(String s) {

        String rev =
            new StringBuilder(s).reverse().toString();

        return lcs(s, rev, s.length(), rev.length());
    }
}
```

---

## Approach 2: Memoization

### Idea

Store already computed states in dp[][]

### Complexity

Time: O(n²)

Space: O(n²)

```java
class Solution {

    int[][] dp;

    int lcs(String s, String rev, int i, int j){

        if(i == 0 || j == 0) return 0;

        if(dp[i][j] != -1) return dp[i][j];

        if(s.charAt(i-1) == rev.charAt(j-1)){
            return dp[i][j] =
                1 + lcs(s, rev, i-1, j-1);
        }

        return dp[i][j] =
            Math.max(
                lcs(s, rev, i-1, j),
                lcs(s, rev, i, j-1)
            );
    }

    public int longestPalindromeSubseq(String s) {

        String rev =
            new StringBuilder(s).reverse().toString();

        int n = s.length();

        dp = new int[n+1][n+1];

        for(int i=0;i<=n;i++){
            java.util.Arrays.fill(dp[i], -1);
        }

        return lcs(s, rev, n, n);
    }
}
```

---

## Approach 3: Bottom-Up DP

### DP State

dp[i][j] = LCS length of

s[0...i-1] and rev[0...j-1]

### Transition

Match:

dp[i][j] = 1 + dp[i-1][j-1]

Mismatch:

dp[i][j] = max(dp[i-1][j], dp[i][j-1])

### Complexity

Time: O(n²)

Space: O(n²)

```java
class Solution {

    public int longestPalindromeSubseq(String s) {

        String rev =
            new StringBuilder(s).reverse().toString();

        int n = s.length();

        int[][] dp = new int[n+1][n+1];

        for(int i=1;i<=n;i++){

            for(int j=1;j<=n;j++){

                if(s.charAt(i-1) == rev.charAt(j-1)){
                    dp[i][j] =
                        1 + dp[i-1][j-1];
                } else {
                    dp[i][j] =
                        Math.max(dp[i-1][j], dp[i][j-1]);
                }
            }
        }

        return dp[n][n];
    }
}
```

---

## Quick Visualization

Example:

s = bbbab

reverse = babbb

LCS = 4

```text
         LCS
        bbbb
       /    \
      /      \
  bbbab      babbb
```

Answer = 4

---

## Interview One-Liner

Longest Palindromic Subsequence

= Longest Common Subsequence between the string and its reverse.

---

## Complexity Summary

| Approach | Time | Space |
|-----------|--------|--------|
| Recursion | Exponential | O(n) |
| Memoization | O(n²) | O(n²) |
| Tabulation | O(n²) | O(n²) |
