# Subset Sum Problem (Recursion → Top-Down → Bottom-Up)

## 🧩 Problem

Given an array and a target sum, determine if a subset exists with that
sum.

------------------------------------------------------------------------

# 1️⃣ Basic Recursion

### 💡 Idea

At each index, we have two choices: - Take the element - Skip the
element

### ⏱ Time Complexity

O(2\^n)

``` java
class Solution {

    static Boolean isSubsetSum(int arr[], int sum) {
        return helper(arr, sum, 0);
    }

    public static Boolean helper(int arr[], int sum, int i){

        if(sum == 0) return true;
        if(i == arr.length || sum < 0) return false;

        return helper(arr, sum - arr[i], i + 1) 
            || helper(arr, sum, i + 1);
    }
}
```

------------------------------------------------------------------------

# 2️⃣ Top-Down DP (Memoization)

### 💡 Idea

Store results of `(i, sum)` to avoid recomputation.

### ⏱ Time Complexity

O(n \* sum)

``` java
class Solution {

    static Boolean isSubsetSum(int arr[], int sum) {
        int n = arr.length;
        Boolean dp[][] = new Boolean[n + 1][sum + 1];
        return helper(arr, sum, 0, dp);
    }

    public static Boolean helper(int arr[], int sum, int i, Boolean[][] dp){

        if(sum == 0) return true;
        if(i == arr.length || sum < 0) return false;

        if(dp[i][sum] != null) return dp[i][sum];

        dp[i][sum] = helper(arr, sum - arr[i], i + 1, dp) 
                  || helper(arr, sum, i + 1, dp);

        return dp[i][sum];
    }
}
```

------------------------------------------------------------------------

# 3️⃣ Bottom-Up DP (Tabulation)

### 💡 Idea

Build solution from smaller subproblems: -
`dp[i][s] = can we form sum s using first i elements`

### ⏱ Time Complexity

O(n \* sum)

``` java
class Solution {

    static Boolean isSubsetSum(int arr[], int sum) {
        int n = arr.length;

        boolean dp[][] = new boolean[n + 1][sum + 1];

        dp[0][0] = true;

        for(int i = 1; i <= n; i++){
            for(int s = 0; s <= sum; s++){

                dp[i][s] = dp[i - 1][s];

                if(s >= arr[i - 1]){
                    dp[i][s] = dp[i][s] || dp[i - 1][s - arr[i - 1]];
                }
            }
        }

        return dp[n][sum];
    }
}
```

------------------------------------------------------------------------

# ⚡ 4️⃣ Space Optimized (1D DP)

``` java
class Solution {

    static Boolean isSubsetSum(int arr[], int sum) {

        boolean dp[] = new boolean[sum + 1];
        dp[0] = true;

        for(int num : arr){
            for(int s = sum; s >= num; s--){
                dp[s] = dp[s] || dp[s - num];
            }
        }

        return dp[sum];
    }
}
```

------------------------------------------------------------------------

# 🧠 Key Takeaways

-   Recursion → exponential
-   Memoization → avoids recomputation
-   Tabulation → iterative + interview friendly
-   1D DP → optimization trick

------------------------------------------------------------------------

# 🚀 Interview Trick

Whenever you see:

    f(i, sum)

Think: - State → `(index, sum)` - Convert → `dp[i][sum]` - Flip →
`first i elements`
