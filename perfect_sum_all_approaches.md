# Perfect Sum Problem - All Approaches

## 1. Recursive Solution

``` java
class Solution {
   
    public int perfectSum(int[] nums, int target) {
        
        return helper(nums, target, 0);
        
    }
    
    public int helper(int[] nums, int target, int i){
        
        if(i==nums.length){
            return (target==0) ? 1 : 0;
        }
        if(target <0){
            return 0;
        }
        
        return helper(nums, target-nums[i], i+1) + helper(nums, target, i+1);
    }
}
```

------------------------------------------------------------------------

## 2. Memoization (Top-Down DP)

``` java
class Solution {
    public int perfectSum(int[] nums, int target) {
        
        int n=nums.length;
        
        Integer[][] dp= new Integer[n+1][target+1];
        
        
        return helper(nums, target, 0, dp);
        
    }
    
    public int helper(int[] nums, int target, int i, Integer[][] dp){
        
        if(i==nums.length){
            return (target==0) ? 1 : 0;
        }
        if(target <0){
            return 0;
        }
        
        if(dp[i][target] != null){
            return dp[i][target];
        }
        
        dp[i][target]= helper(nums, target-nums[i], i+1, dp) + helper(nums, target, i+1, dp);
        
        return dp[i][target];
    }
}
```

------------------------------------------------------------------------

## 3. Bottom-Up (Tabulation)

``` java
class Solution {
    public int perfectSum(int[] arr, int target) {
        int n = arr.length;
        int[][] dp = new int[n+1][target+1];

        dp[0][0] = 1;

        for(int i = 1; i <= n; i++){
            for(int j = 0; j <= target; j++){
                dp[i][j] = dp[i-1][j];

                if(arr[i-1] <= j){
                    dp[i][j] += dp[i-1][j - arr[i-1]];
                }
            }
        }

        return dp[n][target];
    }
}
```

------------------------------------------------------------------------

## 4. Space Optimized (1D DP)

``` java
class Solution {
    public int perfectSum(int[] arr, int target) {
        int[] dp = new int[target + 1];
        dp[0] = 1;

        for(int num : arr){
            for(int j = target; j >= 0; j--){
                if(num <= j){
                    dp[j] += dp[j - num];
                }
            }
        }

        return dp[target];
    }
}
```

------------------------------------------------------------------------

## Key Notes

-   Always handle `target == 0` carefully (especially with zeros in
    array)
-   Use `Integer[][]` for memoization if you want to use null checks
-   Use reverse loop in 1D DP to avoid overwriting
