# Target Sum (DP) --- Revision

## 🧩 Problem

Given an array `arr` and an integer `diff`, assign `+` or `-` signs to
each element such that the final expression equals `diff`.

Return the number of ways.

------------------------------------------------------------------------

## 🔑 Key Idea (Trick)

Let: - `P` = subset with positive signs\
- `N` = subset with negative signs

We have:

    P - N = diff
    P + N = totalSum

Adding:

    2 * P = diff + totalSum
    P = (diff + totalSum) / 2

👉 Reduce problem to:

> **Count subsets with sum = P**

------------------------------------------------------------------------

## ⚠️ Validity Conditions

-   `(diff + totalSum) % 2 == 0`
-   `diff <= totalSum`

Otherwise → answer is `0`

------------------------------------------------------------------------

## 🧠 Approach 1: Pure Recursion

### Code:

``` java
class Solution {
    public int findTargetSumWays(int[] arr, int diff) {
         int sum = 0;
    for (int num : arr) sum += num;

    if ((diff + sum) % 2 != 0 || diff > sum) return 0;

    int reqSum = (diff + sum) / 2;

    return helper(arr, reqSum, 0);
    }
    public int helper(int[] arr, int reqSum, int i){
    if (i == arr.length) return reqSum == 0 ? 1 : 0;

    if (reqSum < 0) return 0;

    return helper(arr, reqSum - arr[i], i + 1) +
           helper(arr, reqSum, i + 1);
}
}
```

### Complexity:

-   Time: `O(2^n)`
-   Not efficient

------------------------------------------------------------------------

## ⚡ Approach 2: Recursion + Memoization

### Code:

``` java
class Solution {
    Integer[][] dp;

    public int findTargetSumWays(int[] arr, int diff) {
        int sum = 0;
        for (int num : arr) sum += num;

        if ((diff + sum) % 2 != 0 || diff > sum) return 0;

        int reqSum = (diff + sum) / 2;

        dp = new Integer[arr.length][reqSum + 1];

        return helper(arr, reqSum, 0);
    }

    public int helper(int[] arr, int reqSum, int i){
        if (i == arr.length) return reqSum == 0 ? 1 : 0;

        if (reqSum < 0) return 0;

        if (dp[i][reqSum] != null) return dp[i][reqSum];

        return dp[i][reqSum] =
            helper(arr, reqSum - arr[i], i + 1) +
            helper(arr, reqSum, i + 1);
    }
}
```

### Complexity:

-   Time: `O(n * sum)`
-   Space: `O(n * sum)`

------------------------------------------------------------------------

## 🚀 Approach 3: Bottom-Up DP (Tabulation)

### Code:

``` java
class Solution {
    public int findTargetSumWays(int[] arr, int diff) {
        int n = arr.length;

        int sum = 0;
        for (int num : arr) sum += num;

        if ((diff + sum) % 2 != 0 || diff > sum) return 0;

        int reqSum = (diff + sum) / 2;

        int[][] dp = new int[n + 1][reqSum + 1];
        dp[0][0] = 1;

        for (int i = 1; i <= n; i++) {
            for (int j = 0; j <= reqSum; j++) {
                dp[i][j] = dp[i - 1][j];

                if (arr[i - 1] <= j) {
                    dp[i][j] += dp[i - 1][j - arr[i - 1]];
                }
            }
        }

        return dp[n][reqSum];
    }
}
```

### Complexity:

-   Time: `O(n * sum)`
-   Space: `O(n * sum)`

------------------------------------------------------------------------

## 🧠 Important Notes

### 1. Zeros Handling

-   If array contains `0`, they double the count

### 2. Core Insight

-   Convert sign assignment → subset sum

------------------------------------------------------------------------

## 🏁 Summary

  Approach      Time Complexity   Space
  ------------- ----------------- -------------
  Recursion     O(2\^n)           O(n)
  Memoization   O(n \* sum)       O(n \* sum)
  Tabulation    O(n \* sum)       O(n \* sum)

------------------------------------------------------------------------

## 🎯 Final Trick

> Convert "target sum" → "subset sum problem"
