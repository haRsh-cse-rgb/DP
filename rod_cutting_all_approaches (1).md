# Rod Cutting Problem

## Problem Statement

Given a rod of length `n` and an array `price[]` where `price[i]` represents the value of a rod piece of length `i + 1`, determine the maximum obtainable value by cutting up the rod and selling the pieces.

This problem is a variation of the **Unbounded Knapsack** problem because a rod piece can be used multiple times.

---

# 1. Recursive Approach (Brute Force)

## Key Idea

For every rod length:

- Either include the current cut length
- Or exclude it

Since the same rod length can be reused multiple times, after including it we stay at the same index.

---

## Code

```java
class Solution {

    public int cutRod(int[] price) {

        int n = price.length;

        int[] arr = new int[n];

        for(int i = 0; i < n; i++){
            arr[i] = i + 1;
        }

        return helper(price, arr, n, n);
    }

    public int helper(int[] price, int[] arr, int n, int i){

        if(n == 0 || i == 0){
            return 0;
        }

        int currentcut = arr[i - 1];
        int currentvalue = price[i - 1];

        if(currentcut > n){

            return helper(price, arr, n, i - 1);

        } else {

            int include =
                currentvalue +
                helper(price, arr, n - currentcut, i);

            int exclude =
                helper(price, arr, n, i - 1);

            return Math.max(include, exclude);
        }
    }
}
```

---

## Complexity

Time Complexity:

O(2^n) approximately

Space Complexity:

O(n)

---

# 2. Memoization Approach (Top-Down DP)

## Key Idea

Store already computed states in a DP table to avoid repeated recursive calls.

State:

`dp[i][n]`

where:

- `i` = number of rod lengths considered
- `n` = remaining rod length

---

## Code

```java
import java.util.Arrays;

class Solution {

    public int cutRod(int[] price) {

        int n = price.length;

        int[] arr = new int[n];

        for(int i = 0; i < n; i++){
            arr[i] = i + 1;
        }

        int[][] dp = new int[n + 1][n + 1];

        for(int i = 0; i <= n; i++){
            Arrays.fill(dp[i], -1);
        }

        return helper(price, arr, n, n, dp);
    }

    public int helper(int[] price, int[] arr,
                      int n, int i, int[][] dp){

        if(n == 0 || i == 0){
            return 0;
        }

        if(dp[i][n] != -1){
            return dp[i][n];
        }

        int currentcut = arr[i - 1];
        int currentvalue = price[i - 1];

        if(currentcut > n){

            return dp[i][n] =
                helper(price, arr, n, i - 1, dp);

        } else {

            int include =
                currentvalue +
                helper(price, arr,
                       n - currentcut, i, dp);

            int exclude =
                helper(price, arr,
                       n, i - 1, dp);

            return dp[i][n] =
                Math.max(include, exclude);
        }
    }
}
```

---

## Complexity

Time Complexity:

O(n^2)

Space Complexity:

O(n^2) + recursion stack

---

# 3. Bottom-Up Approach (Tabulation)

## Key Idea

Build the DP table iteratively.

Transition:

- Include current rod length
- Exclude current rod length

Important:

For inclusion we use:

```java
dp[i][j - length[i - 1]]
```

because rod cutting is an **Unbounded Knapsack** problem.

---

## Code

```java
class Solution {

    public int cutRod(int[] price) {

        int n = price.length;

        int[] length = new int[n];

        for(int i = 0; i < n; i++) {
            length[i] = i + 1;
        }

        // dp[i][j] = max profit using first i rod lengths
        // to make total rod length j
        int[][] dp = new int[n + 1][n + 1];

        // Build the table
        for(int i = 1; i <= n; i++) {

            for(int j = 1; j <= n; j++) {

                // Cannot include current rod length
                if(length[i - 1] > j) {

                    dp[i][j] = dp[i - 1][j];

                } else {

                    int include =
                        price[i - 1]
                        + dp[i][j - length[i - 1]];

                    int exclude =
                        dp[i - 1][j];

                    dp[i][j] =
                        Math.max(include, exclude);
                }
            }
        }

        return dp[n][n];
    }
}
```

---

## Complexity

Time Complexity:

O(n^2)

Space Complexity:

O(n^2)

---

# Important Difference from 0/1 Knapsack

## 0/1 Knapsack

```java
dp[i - 1][remainingWeight]
```

The item cannot be reused.

---

## Rod Cutting / Unbounded Knapsack

```java
dp[i][remainingLength]
```

The same rod piece can be reused multiple times.
