# Target Sum (DP) --- Revision

## 🧩 Problem

Given an array `nums` and an integer `target`, assign either `+` or `-`
sign to each element such that the total sum equals `target`.

Return the number of ways to achieve this.

------------------------------------------------------------------------

## 🔑 Key Idea (Trick)

Let: - `P` = subset with positive signs\
- `N` = subset with negative signs

We have:

    sum(P) - sum(N) = target
    sum(P) + sum(N) = totalSum

Adding:

    2 * sum(P) = target + totalSum
    sum(P) = (target + totalSum) / 2

👉 Reduce problem to:

> **Count subsets with sum = (target + totalSum) / 2**

------------------------------------------------------------------------

## ⚠️ Validity Conditions

-   `(target + totalSum) % 2 == 0`
-   `abs(target) <= totalSum`

Otherwise → answer is `0`

------------------------------------------------------------------------

## 🧠 Approach 1: Pure Recursion

### Idea:

For each element: - Add it (+) - Subtract it (-)

### Code:

``` java
public int findTargetSumWays(int[] nums, int target) {
    return helper(nums, target, 0);
}

public int helper(int[] nums, int target, int i){
    if (i == nums.length) return target == 0 ? 1 : 0;

    return helper(nums, target - nums[i], i + 1) +
           helper(nums, target + nums[i], i + 1);
}
```

### Complexity:

-   Time: `O(2^n)`
-   Not efficient

------------------------------------------------------------------------

## ⚡ Approach 2: Recursion + Memoization

### Idea:

Store overlapping subproblems.

### Code:

``` java
Integer[][] dp;

public int findTargetSumWays(int[] nums, int target) {
    int sum = 0;
    for (int num : nums) sum += num;

    if ((target + sum) % 2 != 0 || Math.abs(target) > sum) return 0;

    int reqSum = (target + sum) / 2;

    dp = new Integer[nums.length][reqSum + 1];

    return helper(nums, reqSum, 0);
}

public int helper(int[] nums, int reqSum, int i){
    if (i == nums.length) return reqSum == 0 ? 1 : 0;

    if (reqSum < 0) return 0;

    if (dp[i][reqSum] != null) return dp[i][reqSum];

    return dp[i][reqSum] =
        helper(nums, reqSum - nums[i], i + 1) +
        helper(nums, reqSum, i + 1);
}
```

### Complexity:

-   Time: `O(n * sum)`
-   Space: `O(n * sum)`

------------------------------------------------------------------------

## 🚀 Approach 3: Bottom-Up DP (Tabulation)

### Idea:

Classic subset sum count.

### Code:

``` java
public int findTargetSumWays(int[] nums, int target) {
    int n = nums.length;

    int sum = 0;
    for (int num : nums) sum += num;

    if ((target + sum) % 2 != 0 || Math.abs(target) > sum) return 0;

    int reqSum = (target + sum) / 2;

    int[][] dp = new int[n + 1][reqSum + 1];
    dp[0][0] = 1;

    for (int i = 1; i <= n; i++) {
        for (int j = 0; j <= reqSum; j++) {
            dp[i][j] = dp[i - 1][j];

            if (nums[i - 1] <= j) {
                dp[i][j] += dp[i - 1][j - nums[i - 1]];
            }
        }
    }

    return dp[n][reqSum];
}
```

### Complexity:

-   Time: `O(n * sum)`
-   Space: `O(n * sum)`

------------------------------------------------------------------------

## 🧠 Important Notes

### 1. Zeros Handling

-   If array contains `0`, they double the count:
    -   +0 or -0 → same contribution

### 2. Subset Transformation

-   This is not obvious brute force → key trick is transformation to
    subset sum

### 3. Same Core as Partition Difference

-   Both reduce to subset sum

------------------------------------------------------------------------

## 🧪 Example

    nums = [1,1,2,3]
    target = 1

    totalSum = 7
    reqSum = (7 + 1) / 2 = 4

Subsets with sum = 4: - {1,3} - {1,3} - {1,1,2}

Answer = 3

------------------------------------------------------------------------

## 🏁 Summary

  Approach      Time Complexity   Space
  ------------- ----------------- -------------
  Recursion     O(2\^n)           O(n)
  Memoization   O(n \* sum)       O(n \* sum)
  Tabulation    O(n \* sum)       O(n \* sum)

------------------------------------------------------------------------

## 🎯 Final Trick

> Convert "+ / - assignment problem" → "subset sum problem"

That's the key interview insight 🚀
