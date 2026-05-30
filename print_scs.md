# LeetCode 1092 - Shortest Common Supersequence (SCS)

## Key Idea

SCS is closely related to LCS.

Characters that belong to the LCS can be shared between both strings.
Characters not in the LCS must appear separately.

## Length Formula

SCS Length = len(str1) + len(str2) - LCS Length

Important:
- This formula works only for the LENGTH.
- It does NOT construct the SCS string.

## Why 'str1 + str2 - LCS' Fails

Example:

str1 = abac
str2 = cab

LCS = ab

Concatenation:

abaccab

Removing 'ab' gives:

accab

But 'abac' is no longer a subsequence of accab.

Therefore it is not a valid SCS.

The formula gives the correct count of characters but not their positions.

## DP Definition

dp[i][j] = LCS length of

str1[0...i-1]
str2[0...j-1]

## Transition

If characters match:

dp[i][j] = 1 + dp[i-1][j-1]

Otherwise:

dp[i][j] = max(dp[i-1][j], dp[i][j-1])

## Example DP Table

str1 = abac
str2 = cab

        ""  c  a  b
      ----------------
""  |  0  0  0  0
a   |  0  0  1  1
b   |  0  0  1  2
a   |  0  0  1  2
c   |  0  1  1  2

LCS Length = 2

## Backtracking Rules

1. Characters match
   - Add once
   - Move diagonally

2. Move up
   - Add character from str1

3. Move left
   - Add character from str2

## Construction Example

Start at (4,3)

c != b
append(c)

Current: c

a != b
append(a)

Current: ca

b == b
append(b)

Current: cab

a == a
append(a)

Current: caba

Remaining character from str2:

append(c)

Current: cabac

Reverse and return.

Answer: cabac

## Time Complexity

DP Build: O(n * m)

Backtracking: O(n + m)

Total: O(n * m)

## Space Complexity

O(n * m)

## Complete Java Solution

```java
class Solution {
    public String shortestCommonSupersequence(String str1, String str2) {

        int n = str1.length();
        int m = str2.length();

        int[][] dp = new int[n + 1][m + 1];

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {

                if (str1.charAt(i - 1) == str2.charAt(j - 1)) {
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j],
                                        dp[i][j - 1]);
                }
            }
        }

        StringBuilder ans = new StringBuilder();

        int i = n;
        int j = m;

        while (i > 0 && j > 0) {

            if (str1.charAt(i - 1) == str2.charAt(j - 1)) {

                ans.append(str1.charAt(i - 1));
                i--;
                j--;

            } else if (dp[i - 1][j] > dp[i][j - 1]) {

                ans.append(str1.charAt(i - 1));
                i--;

            } else {

                ans.append(str2.charAt(j - 1));
                j--;
            }
        }

        while (i > 0) {
            ans.append(str1.charAt(i - 1));
            i--;
        }

        while (j > 0) {
            ans.append(str2.charAt(j - 1));
            j--;
        }

        return ans.reverse().toString();
    }
}
```
