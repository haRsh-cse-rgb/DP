# Coin Change II — Recursion, Memoization, Bottom-Up DP

# Problem

Given an integer array `coins` representing different denominations and an integer `amount`,
return the number of combinations that make up that amount.

You may use each coin unlimited times.

---

# 1. Pure Recursion

## Key Idea

At every index:

- Take the current coin
- Skip the current coin

Because coins can be reused unlimited times:

- When taking coin → stay at same index
- When skipping coin → move to next index

---

## Recursive Tree Logic

```text
f(amount, i)
|
|-- take coin[i]  -> f(amount - coin[i], i)
|
|-- skip coin[i]  -> f(amount, i+1)
```

---

## Code

```java
class Solution {

    public int change(int amount, int[] coins) {
        return helper(amount, coins, 0);
    }

    public int helper(int amount, int[] coins, int i) {

        if (amount == 0) {
            return 1;
        }

        if (i == coins.length || amount < 0) {
            return 0;
        }

        int take = helper(amount - coins[i], coins, i);

        int skip = helper(amount, coins, i + 1);

        return take + skip;
    }
}
```

---

## Time Complexity

Exponential

```text
O(2^N)
```

because many states repeat.

---

## Space Complexity

```text
O(amount)
```

(recursion stack)

---

## Trick

For unbounded problems:

```text
take -> stay at same index
```

For 0/1 knapsack:

```text
take -> move to next index
```

---

# 2. Memoization (Top Down DP)

## Key Idea

Many recursive states repeat.

Store:

```text
dp[i][amount]
```

meaning:

```text
number of ways to form amount using coins from index i
```

---

## Code

```java
class Solution {

    public int change(int amount, int[] coins) {

        Integer[][] dp = new Integer[coins.length][amount + 1];

        return helper(amount, coins, 0, dp);
    }

    public int helper(int amount, int[] coins, int i, Integer[][] dp) {

        if (amount == 0) {
            return 1;
        }

        if (i == coins.length || amount < 0) {
            return 0;
        }

        if (dp[i][amount] != null) {
            return dp[i][amount];
        }

        int take = helper(amount - coins[i], coins, i, dp);

        int skip = helper(amount, coins, i + 1, dp);

        dp[i][amount] = take + skip;

        return dp[i][amount];
    }
}
```

---

## Time Complexity

```text
O(N * amount)
```

because each state computed once.

---

## Space Complexity

```text
O(N * amount) + recursion stack
```

---

## Important Trick

This is the MOST IMPORTANT observation:

```text
State = (index, amount)
```

Never include `count` in DP state.

Because:

```text
count is derived value
```

not a changing state parameter.

---

# 3. Bottom Up DP (Tabulation)

## Key Idea

Build DP table iteratively.

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

There is exactly ONE way to make amount 0:

```text
choose nothing
```

---

# Transition

## Not Take Coin

```text
dp[i-1][j]
```

## Take Coin

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

## Code

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

## Key Idea

Current row depends on:

- previous row
- current row itself

So 1D DP works.

---

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

# Most Important Interview Tricks

## 1. Combination vs Permutation

This problem asks:

```text
combinations
```

Meaning:

```text
1+2 and 2+1 are SAME
```

That is why:

```java
for coin
    for amount
```

If loops reverse:

```java
for amount
    for coin
```

then permutations counted.

---

# 2. Unbounded Knapsack Pattern

Recognize by keywords:

- unlimited coins
- infinite supply
- reuse allowed

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

If choices depend on:

- current index
- remaining amount

then DP state:

```text
(index, amount)
```

---

# Final Pattern Summary

| Approach | Time | Space |
|---|---|---|
| Recursion | Exponential | O(amount) |
| Memoization | O(N * amount) | O(N * amount) |
| Tabulation | O(N * amount) | O(N * amount) |
| Space Optimized | O(N * amount) | O(amount) |

