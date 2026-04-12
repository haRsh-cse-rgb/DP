# 🧩 Partition Equal Subset Sum

## 🎯 Problem
Given an array `nums`, return `true` if you can partition it into two subsets such that the sum of elements in both subsets is equal.

---

# 🧠 Key Idea

- Total sum must be even
- Target = sum / 2
- Problem reduces to:  
  👉 "Can we find a subset with sum = target?"

---

# 🔁 1. Recursion (Brute Force)

## 💡 Idea:
At each index:
- Take the element
- Skip the element

---

## ✅ Code

```java
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for(int num : nums) sum += num;

        if(sum % 2 != 0) return false;

        return helper(nums, sum / 2, 0);
    }

    public boolean helper(int[] nums, int sum, int i){
        if(sum == 0) return true;
        if(i == nums.length || sum < 0) return false;

        return helper(nums, sum - nums[i], i + 1) 
            || helper(nums, sum, i + 1);
    }
}
```

---

## ⏱ Complexity
- Time: **O(2^n)**
- Space: **O(n)** (recursion stack)

---

# ⚡ 2. Recursion + Memoization (Top-Down DP)

## 💡 Idea:
Store already computed states:
👉 `dp[i][sum]`

---

## ✅ Code

```java
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for(int num : nums) sum += num;

        if(sum % 2 != 0) return false;

        Boolean[][] dp = new Boolean[nums.length][sum/2 + 1];

        return helper(nums, sum / 2, 0, dp);
    }

    public boolean helper(int[] nums, int sum, int i, Boolean[][] dp){
        if(sum == 0) return true;
        if(i == nums.length || sum < 0) return false;

        if(dp[i][sum] != null) return dp[i][sum];

        boolean take = helper(nums, sum - nums[i], i + 1, dp);
        boolean skip = helper(nums, sum, i + 1, dp);

        return dp[i][sum] = take || skip;
    }
}
```

---

## ⏱ Complexity
- Time: **O(n × sum)**
- Space: **O(n × sum)**

---

# 📊 3. Bottom-Up DP (Tabulation)

## 💡 Idea:
Build a table:

👉 `dp[i][j] = can we make sum j using first i elements?`

---

## ✅ Code

```java
class Solution {
    public boolean canPartition(int[] nums) {
        int n = nums.length;

        int sum = 0;
        for(int num : nums){
            sum += num;
        }

        if(sum % 2 != 0){
            return false;
        }

        sum = sum / 2;

        boolean[][] dp = new boolean[n+1][sum+1];

        dp[0][0] = true;

        for(int i = 1; i <= n; i++){
            for(int j = 0; j <= sum; j++){

                dp[i][j] = dp[i-1][j];

                if(nums[i-1] <= j){
                    dp[i][j] = dp[i][j] || dp[i-1][j - nums[i-1]];
                }
            }
        }

        return dp[n][sum];
    }
}
```

---

## ⏱ Complexity
- Time: **O(n × sum)**
- Space: **O(n × sum)**

---

# 🧠 Key Takeaways

- Recursion → exponential
- Memoization → optimized recursion
- Tabulation → iterative + efficient

---

# 🚀 Bonus Insight

```
dp[i][j] =
    dp[i-1][j]
    OR
    dp[i-1][j - nums[i-1]]
```

---

# 🔥 Final Tip

If you see:
👉 Pick / Not Pick

Think:
👉 Subset DP
