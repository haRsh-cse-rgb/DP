# Count of Subsets with Given Difference (DP) — Revision

## 🧩 Problem
Given an array `arr` and an integer `diff`, count the number of ways to partition the array into two subsets such that:

```
|sum(S1) - sum(S2)| = diff
```

---

## 🔑 Key Idea (Trick)

Let:
- `s1` = sum of subset 1  
- `s2` = sum of subset 2  
- `totalSum` = sum of all elements  

We have:
```
s1 - s2 = diff
s1 + s2 = totalSum
```

Adding:
```
2 * s1 = diff + totalSum
s1 = (diff + totalSum) / 2
```

👉 Reduce problem to:

> **Count subsets with sum = s1**

---

## ⚠️ Validity Conditions

- `(diff + totalSum) % 2 == 0`
- `diff <= totalSum`

Otherwise → answer is `0`

---

## 🧠 Approach 1: Pure Recursion

### Idea:
Try:
- Take element  
- Not take element  

### Code:
```java
public int countPartitions(int[] arr, int diff) {
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
```

### Complexity:
- Time: `O(2^n)`
- Not efficient

---

## ⚡ Approach 2: Recursion + Memoization

### Idea:
Store results of overlapping subproblems.

### Code:
```java
Integer[][] dp;

public int countPartitions(int[] arr, int diff) {
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
```

### Complexity:
- Time: `O(n * reqSum)`
- Space: `O(n * reqSum)`

---

## 🚀 Approach 3: Bottom-Up DP (Tabulation)

### Idea:
Classic subset sum count.

### Code:
```java
public int countPartitions(int[] arr, int diff) {
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
```

### Complexity:
- Time: `O(n * reqSum)`
- Space: `O(n * reqSum)`

---

## 🧠 Important Notes

### 1. Zeros Handling
- If array contains `0`, they double the count:
  - Include or exclude → same sum

### 2. Order doesn’t matter
- Subsets, not permutations

### 3. This is a Partition Problem
- Must use all elements
- Otherwise formula breaks

---

## 🧪 Example

```
arr = [1, 1, 2, 3]
diff = 1

totalSum = 7
s1 = (7 + 1) / 2 = 4
```

Count subsets with sum = 4:
- {1,3}
- {1,3} (other 1)
- {1,1,2}

Answer = 3

---

## 🏁 Summary

| Approach        | Time Complexity | Space |
|----------------|---------------|-------|
| Recursion      | O(2^n)        | O(n)  |
| Memoization    | O(n * sum)    | O(n * sum) |
| Tabulation     | O(n * sum)    | O(n * sum) |

---

## 🎯 Final Trick

> Convert “difference problem” → “subset sum problem”

That’s the key interview insight 🚀
