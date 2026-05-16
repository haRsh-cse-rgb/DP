# Coin Change - Complete Notes

# Problem

Given an integer array `coins` representing different coin denominations and an integer `amount`, return the minimum number of coins needed to make up that amount.

If it is not possible to form the amount, return `-1`.

---

# Key Idea

At every index we have 2 choices:

1. TAKE the current coin
2. NOT TAKE the current coin

Since we can use the same coin multiple times:

```text
TAKE -> stay on same index
```

That is the main trick.

---

# Recursive Approach (Brute Force)

## Code

```java
class Solution {

    public int coinChange(int[] coins, int amount) {

        int ans = helper(coins, amount, 0);

        return ans == Integer.MAX_VALUE ? -1 : ans;
    }

    public int helper(int[] coins, int amount, int i) {

        // Amount formed
        if (amount == 0) {
            return 0;
        }

        // Invalid cases
        if (i == coins.length || amount < 0) {
            return Integer.MAX_VALUE;
        }

        // TAKE current coin
        int take = helper(coins, amount - coins[i], i);

        if (take != Integer.MAX_VALUE) {
            take = take + 1;
        }

        // NOT TAKE current coin
        int notTake = helper(coins, amount, i + 1);

        return Math.min(take, notTake);
    }
}
```

---

# Recursive Thinking

State:

```text
helper(i, amount)
```

Meaning:

```text
Minimum coins needed to form amount
using coins from index i onward
```

---

# Why TAKE stays at same index

```text
take = helper(i, amount - coins[i])
```

We stay at same index because:

```text
Same coin can be reused unlimited times
```

If coin could be used only once:

```text
take = helper(i + 1, amount - coins[i])
```

---

# Base Cases

## Amount becomes 0

```java
if(amount == 0){
    return 0;
}
```

Meaning:

```text
Amount already formed
No more coins needed
```

---

## Invalid case

```java
if(i == coins.length || amount < 0){
    return Integer.MAX_VALUE;
}
```

Meaning:

```text
Impossible path
```

---

# Why we add +1

```java
take = take + 1;
```

Because:

```text
1 -> current coin taken
take -> coins needed for remaining amount
```

---

# Why Integer.MAX_VALUE check is needed

Without check:

```java
Integer.MAX_VALUE + 1
```

causes overflow.

So:

```java
if(take != Integer.MAX_VALUE){
    take = take + 1;
}
```

---

# Time Complexity (Brute Force)

```text
Exponential
```

Approximately:

```text
O(2^amount)
```

because many states repeat again and again.

---

# Memoization (Top Down DP)

## Idea

Store already solved states.

State:

```text
(i, amount)
```

---

## Code

```java
class Solution {

    public int coinChange(int[] coins, int amount) {

        int n = coins.length;

        Integer[][] dp = new Integer[n + 1][amount + 1];

        int ans = helper(coins, amount, 0, dp);

        return ans == Integer.MAX_VALUE ? -1 : ans;
    }

    public int helper(int[] coins, int amount, int i, Integer[][] dp){

        if(amount == 0){
            return 0;
        }

        if(i == coins.length || amount < 0){
            return Integer.MAX_VALUE;
        }

        if(dp[i][amount] != null){
            return dp[i][amount];
        }

        int take = helper(coins, amount - coins[i], i, dp);

        if(take != Integer.MAX_VALUE){
            take = take + 1;
        }

        int notTake = helper(coins, amount, i + 1, dp);

        dp[i][amount] = Math.min(take, notTake);

        return dp[i][amount];
    }
}
```

---

# Memoization State

```text
dp[i][amount]
```

Meaning:

```text
Minimum coins needed to form amount
using coins from index i onward
```

---

# Time Complexity

```text
O(n * amount)
```

Because each state is solved only once.

---

# Bottom Up DP (2D)

## Conversion

Recursive state:

```text
helper(i, amount)
```

becomes:

```text
dp[i][amount]
```

---

# Code

```java
class Solution {

    public int coinChange(int[] coins, int amount) {

        int n = coins.length;

        int[][] dp = new int[n + 1][amount + 1];

        int INF = (int)1e9;

        // Last row initialization
        for(int a = 1; a <= amount; a++){
            dp[n][a] = INF;
        }

        for(int i = n - 1; i >= 0; i--){

            for(int a = 0; a <= amount; a++){

                // NOT TAKE
                int notTake = dp[i + 1][a];

                // TAKE
                int take = INF;

                if(a - coins[i] >= 0){
                    take = 1 + dp[i][a - coins[i]];
                }

                dp[i][a] = Math.min(take, notTake);
            }
        }

        return dp[0][amount] >= INF ? -1 : dp[0][amount];
    }
}
```

---

# Why TAKE uses same row

```java
take = 1 + dp[i][a - coins[i]];
```

Notice:

```text
same row i
```

because:

```text
Coin can be reused unlimited times
```

---

# Why use 1e9 instead of Integer.MAX_VALUE

```java
int INF = (int)1e9;
```

Meaning:

```text
1000000000
```

If we use:

```java
Integer.MAX_VALUE
```

then:

```java
1 + Integer.MAX_VALUE
```

causes overflow.

Example:

```java
System.out.println(Integer.MAX_VALUE + 1);
```

Output:

```text
-2147483648
```

So in DP:

```text
Use 1e9 as safe infinity
```

---

# Complexity Comparison

| Approach | Time Complexity | Space Complexity |
|---|---|---|
| Recursive | Exponential | O(amount) recursion stack |
| Memoization | O(n * amount) | O(n * amount) |
| Bottom Up | O(n * amount) | O(n * amount) |

---

# Important Tricks

## Trick 1

If coin can be reused:

```text
TAKE -> stay at same index
```

---

## Trick 2

If using `Integer.MAX_VALUE`:

Always check before adding:

```java
if(take != Integer.MAX_VALUE)
```

---

## Trick 3

Bottom up conversion:

```text
helper(i, amount)
```

directly becomes:

```text
dp[i][amount]
```

---

# Final Intuition

At every state:

```text
Either take current coin
or skip current coin
```

Then choose minimum.

```text
Answer = min(take, notTake)
```
