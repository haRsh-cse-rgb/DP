# Matrix Chain Multiplication (MCM)

## Intuition

Given dimensions array:

```text
arr = [10, 30, 5, 60]
```

Matrices:

```text
A1 = 10 x 30
A2 = 30 x 5
A3 = 5 x 60
```

The order of multiplication affects the number of scalar multiplications.

Example:

```text
(A1A2)A3 = 4500
A1(A2A3) = 27000
```

Goal: Find the minimum cost.

---

# 1. Recursive Solution

### Recurrence

```text
mcm(i, j) =
min over all k

mcm(i, k)
+ mcm(k+1, j)
+ arr[i-1] * arr[k] * arr[j]
```

### Base Case

```java
if(i >= j) return 0;
```

### Code

```java
class Solution {

    static int matrixMultiplication(int arr[]) {
        int n = arr.length;
        return mcm(arr, 1, n - 1);
    }

    static int mcm(int arr[], int i, int j) {

        if (i >= j) {
            return 0;
        }

        int result = Integer.MAX_VALUE;

        for (int k = i; k < j; k++) {

            int score =
                    mcm(arr, i, k)
                  + mcm(arr, k + 1, j)
                  + arr[i - 1] * arr[k] * arr[j];

            result = Math.min(result, score);
        }

        return result;
    }
}
```

### Complexity

- Time: Exponential
- Space: O(n) recursion stack

---

# 2. Memoization (Top-Down DP)

### Idea

Store results of already solved states:

```java
dp[i][j]
```

so that every interval is computed only once.

### Code

```java
class Solution {

    static int matrixMultiplication(int arr[]) {

        int n = arr.length;

        int[][] dp = new int[n][n];

        for(int i = 0; i < n; i++) {
            for(int j = 0; j < n; j++) {
                dp[i][j] = -1;
            }
        }

        return mcm(arr, 1, n - 1, dp);
    }

    static int mcm(int[] arr, int i, int j, int[][] dp) {

        if(i >= j) {
            return 0;
        }

        if(dp[i][j] != -1) {
            return dp[i][j];
        }

        int result = Integer.MAX_VALUE;

        for(int k = i; k < j; k++) {

            int score =
                    mcm(arr, i, k, dp)
                  + mcm(arr, k + 1, j, dp)
                  + arr[i - 1] * arr[k] * arr[j];

            result = Math.min(result, score);
        }

        return dp[i][j] = result;
    }
}
```

### Complexity

- Time: O(n^3)
- Space: O(n^2) + O(n) recursion stack

---

# 3. Bottom-Up DP (Tabulation)

## Key Intuition

dp[i][j] = minimum cost to multiply matrices from i to j.

A single matrix requires no multiplication:

```java
dp[i][i] = 0;
```

Fill smaller intervals first, then larger intervals.

### Filling Order

```text
dp[1][1] dp[2][2] dp[3][3]

dp[1][2] dp[2][3]

dp[1][3]
```

Think of it as filling diagonally.

### What is len?

```java
len = number of matrices in the interval
```

For:

```text
A1 A2 A3
```

When:

```text
len = 2
```

compute:

```text
dp[1][2]
dp[2][3]
```

When:

```text
len = 3
```

compute:

```text
dp[1][3]
```

### Code

```java
class Solution {

    static int matrixMultiplication(int arr[]) {

        int n = arr.length;

        int[][] dp = new int[n][n];

        for(int len = 2; len < n; len++) {

            for(int i = 1; i <= n - len; i++) {

                int j = i + len - 1;

                dp[i][j] = Integer.MAX_VALUE;

                for(int k = i; k < j; k++) {

                    int cost =
                            dp[i][k]
                          + dp[k + 1][j]
                          + arr[i - 1] * arr[k] * arr[j];

                    dp[i][j] = Math.min(dp[i][j], cost);
                }
            }
        }

        return dp[1][n - 1];
    }
}
```

### Complexity

- Time: O(n^3)
- Space: O(n^2)

---

# Interview Notes

### Must Know

- Recurrence derivation
- Recursive solution
- Memoization
- Complexity analysis

### Good to Know

- Bottom-up conversion
- Why intervals are filled by increasing length

### Common Pattern

Matrix Chain Multiplication is the foundation of:

- Burst Balloons
- Minimum Cost to Cut a Stick
- Boolean Parenthesization
- Optimal BST

All of these are examples of Interval / Partition DP.
