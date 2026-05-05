# Minimum Subset Sum Difference (Partition Problem)

## 🔹 Problem
Given an array, partition it into two subsets such that the difference of their sums is minimized.

---

## 🧠 Key Formula
Difference = | totalSum - 2 × subsetSum |

---

# 1️⃣ Recursion + Memoization (Top-Down)

```java
class Solution {
    public int minDifference(int arr[]) {
        int n = arr.length;
        int sum = 0;

        for (int num : arr) {
            sum += num;
        }

        Integer[][] dp = new Integer[n + 1][sum + 1];

        return helper(arr, dp, sum, 0, 0);
    }

    public int helper(int arr[], Integer[][] dp, int sum, int i, int currentSum) {
        if (i == arr.length) {
            return Math.abs(sum - 2 * currentSum);
        }

        if (dp[i][currentSum] != null) {
            return dp[i][currentSum];
        }

        int include = helper(arr, dp, sum, i + 1, currentSum + arr[i]);
        int exclude = helper(arr, dp, sum, i + 1, currentSum);

        return dp[i][currentSum] = Math.min(include, exclude);
    }
}
```

### ⏱ Complexity
- Time: O(n × sum)
- Space: O(n × sum) + recursion stack

---

# 2️⃣ Bottom-Up (Tabulation)

```java
class Solution {
    public int minDifference(int arr[]) {
        int n = arr.length;
        int sum = 0;

        for (int num : arr) {
            sum += num;
        }

        boolean[][] dp = new boolean[n + 1][sum + 1];

        for (int i = 0; i <= n; i++) {
            dp[i][0] = true;
        }

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= sum; j++) {

                dp[i][j] = dp[i - 1][j];

                if (arr[i - 1] <= j) {
                    dp[i][j] = dp[i][j] || dp[i - 1][j - arr[i - 1]];
                }
            }
        }

        int minDiff = Integer.MAX_VALUE;

        for (int s1 = 0; s1 <= sum / 2; s1++) {
            if (dp[n][s1]) {
                minDiff = Math.min(minDiff, sum - 2 * s1);
            }
        }

        return minDiff;
    }
}
```

### ⏱ Complexity
- Time: O(n × sum)
- Space: O(n × sum)

---

# 3️⃣ Space Optimized (Best)

```java
class Solution {
    public int minDifference(int arr[]) {
        int sum = 0;

        for (int num : arr) {
            sum += num;
        }

        boolean[] dp = new boolean[sum + 1];
        dp[0] = true;

        for (int num : arr) {
            for (int j = sum; j >= num; j--) {
                dp[j] = dp[j] || dp[j - num];
            }
        }

        int minDiff = Integer.MAX_VALUE;

        for (int s1 = 0; s1 <= sum / 2; s1++) {
            if (dp[s1]) {
                minDiff = Math.min(minDiff, sum - 2 * s1);
            }
        }

        return minDiff;
    }
}
```

### ⏱ Complexity
- Time: O(n × sum)
- Space: O(sum)

---

# ⚡ Revision Tricks

## 🔹 Core Idea
- Convert problem into subset sum
- Only check till sum/2

## 🔹 Important Patterns
- Include / Exclude → DP transition
- Reverse loop in 1D DP (avoid overwrite)
- Always use arr[i-1] in tabulation

## 🔹 Common Mistakes
- Using arr[i] instead of arr[i-1]
- Wrong loop bounds (<= vs <)
- Not initializing dp[i][0] = true

## 🔹 When to Use
- Partition problems
- Equal subset sum
- Minimum difference
- Target sum variants

---

# 🚀 Interview Tip
If you see:
- "Partition into 2 subsets"
- "Minimum difference"

👉 Think: Subset Sum + DP

---

