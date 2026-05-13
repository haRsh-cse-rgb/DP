# Coin Change II — Recursion, Memoization, Bottom-Up DP

# Problem

Given an integer array `coins` and an integer `amount`,
return the number of combinations that make up that amount.

Each coin can be used unlimited times.

---

# 1. Pure Recursion

## Key Idea

At every step:

- Take current coin
- Skip current coin

Since coins are reusable:

```text
take -> stay at same index
skip -> move to next index
```

---

## Recursion Code

```java
class Solution {
    public int change(int amount, int[] coins) {

        int count = 0;

        return helper(amount, coins, 0, count);
    }

    public int helper(int amount, int[] coins, int i, int count) {

        if (i == coins.length || amount < 0) {
            return 0;
        }

        if (amount == 0) {
            count++;
            return count;
        }

        return helper(amount - coins[i], coins, i, count)
                +
               helper(amount, coins, i + 1, count);
    }
}
```

---

## Time Complexity

```text
Exponential
```

---

## Space Complexity

```text
O(amount)
```

(recursion stack)

---

## Important Trick

For unbounded problems:

```text
take -> same index
```

For 0/1 knapsack:

```text
take -> next index
```

---

# 2. Memoization (Top Down DP)

## Key Idea

Many recursive states repeat.

Store result of:

```text
(index, amount)
```

inside DP table.

---

## Memoization Code

```java
class Solution {
    public int change(int amount, int[] coins) {

        int count = 0;
        int n = coins.length;

        Integer[][] dp = new Integer[n + 1][amount + 1];

        return helper(amount, coins, 0, count, dp);
    }

    public int helper(int amount, int[] coins, int i, int count, Integer[][] dp) {

        if (i == coins.length || amount < 0) {
            return 0;
        }

        if (amount == 0) {
            count++;
            return count;
        }

        if (dp[i][amount] != null) {
            return dp[i][amount];
        }

        dp[i][amount] =
                helper(amount - coins[i], coins, i, count, dp)
                +
                helper(amount, coins, i + 1, count, dp);

        return dp[i][amount];
    }
}
```

---

## Time Complexity

```text
O(N * amount)
```

---

## Space Complexity

```text
O(N * amount)
```

---

## Important Observation

DP state depends on:

```text
(index, amount)
```

`count` is NOT part of state.

It is only used for returning final combinations.

---

# 3. Bottom Up DP (Tabulation)

## Key Idea

Define:

```text
dp[i][j]
```

= number of ways to make amount `j`
using first `i` coins.

---

# Base Case

```java
dp[i][0] = 1
```

Why?

Because there is exactly ONE way to make amount 0:

```text
choose nothing
```

---

# Transition

## Not Take Current Coin

```text
dp[i-1][j]
```

## Take Current Coin

```text
dp[i][j - coins[i-1]]
```

Why same row?

Because coin can be reused unlimited times.

---

## Formula

```text
dp[i][j]
=
dp[i-1][j]
+
dp[i][j - coins[i-1]]
```

---

## Bottom Up Code

```java
class Solution {
    public int change(int amount, int[] coins) {

        int n = coins.length;

        int[][] dp = new int[n + 1][amount + 1];

        for (int i = 0; i <= n; i++) {
            dp[i][0] = 1;
        }

        for (int i = 1; i <= n; i++) {

            for (int j = 0; j <= amount; j++) {

                dp[i][j] = dp[i - 1][j];

                if (j >= coins[i - 1]) {
                    dp[i][j] += dp[i][j - coins[i - 1]];
                }
            }
        }

        return dp[n][amount];
    }
}
```

---

## Time Complexity

```text
O(N * amount)
```

---

## Space Complexity

```text
O(N * amount)
```

---

# 4. Space Optimized DP

## Code

```java
class Solution {

    public int change(int amount, int[] coins) {

        int[] dp = new int[amount + 1];

        dp[0] = 1;

        for (int coin : coins) {

            for (int j = coin; j <= amount; j++) {

                dp[j] += dp[j - coin];
            }
        }

        return dp[amount];
    }
}
```

---

## Time Complexity

```text
O(N * amount)
```

---

## Space Complexity

```text
O(amount)
```

---

# Important Interview Tricks

## 1. Combination vs Permutation

This problem counts:

```text
combinations
```

Meaning:

```text
1+2 and 2+1 are same
```

That is why loops are:

```java
for coin
    for amount
```

If reversed:

```java
for amount
    for coin
```

then permutations are counted.

---

# 2. Unbounded Knapsack Pattern

Keywords:

- unlimited supply
- reusable coins
- infinite usage

Transition:

```text
take -> same index
```

---

# 3. Why dp[i][0] = 1 ?

Because:

```text
Amount 0 can always be formed
by choosing nothing.
```

---

# 4. State Recognition Trick

If recursive choices depend on:

- current index
- remaining amount

then DP state:

```text
(index, amount)
```

---

# Final Complexity Summary

| Approach | Time | Space |
|---|---|---|
| Recursion | Exponential | O(amount) |
| Memoization | O(N * amount) | O(N * amount) |
| Tabulation | O(N * amount) | O(N * amount) |
| Space Optimized | O(N * amount) | O(amount) |

