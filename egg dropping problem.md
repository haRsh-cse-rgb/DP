# Egg Dropping Problem (LeetCode 887 - Super Egg Drop)

## Problem Statement

You are given **k** identical eggs and a building with **n** floors.

There exists an unknown floor **f** (`0 <= f <= n`) such that:

-   Dropping an egg from a floor **\<= f** does **not** break it.
-   Dropping an egg from a floor **\> f** **breaks** it.

Return the **minimum number of moves** required to determine the exact
value of `f` in the **worst case**.

------------------------------------------------------------------------

# Key Idea

For every chosen floor `i`:

-   **Egg breaks**
    -   Remaining eggs = `k-1`
    -   Remaining floors = `i-1`
-   **Egg survives**
    -   Remaining eggs = `k`
    -   Remaining floors = `n-i`

Therefore,

    dp[k][n] =
    min over all i(
        1 + max(
            dp[k-1][i-1],
            dp[k][n-i]
        )
    )

Why `max`?

Because we must guarantee the answer in the **worst case**.

Why `min`?

Because we are free to choose the first floor optimally.

------------------------------------------------------------------------

# Solution 1 - Recursive + Memoization (O(k\*n²))

``` java
class Solution {
    Integer[][] dp = new Integer[101][10001];

    public int superEggDrop(int k, int n) {

        if (n == 0 || n == 1)
            return n;

        if (k == 1)
            return n;

        if (dp[k][n] != null)
            return dp[k][n];

        int result = Integer.MAX_VALUE;

        for (int i = 1; i <= n; i++) {

            int temp = 1 + Math.max(
                    superEggDrop(k - 1, i - 1),
                    superEggDrop(k, n - i));

            result = Math.min(result, temp);
        }

        return dp[k][n] = result;
    }
}
```

### Complexity

-   Time: **O(k × n²)**
-   Space: **O(k × n)**

------------------------------------------------------------------------

# Solution 2 - DP + Binary Search (Optimized)

Observation:

For a fixed `(k,n)`,

    low = dp[k-1][i-1]

is increasing as `i` increases.

    high = dp[k][n-i]

is decreasing as `i` increases.

Therefore,

    max(low, high)

has one minimum.

Instead of checking every floor, binary search finds the best `i`.

``` java
class Solution {

    Integer[][] dp = new Integer[101][10001];

    public int superEggDrop(int k, int n) {

        if (n == 0 || n == 1)
            return n;

        if (k == 1)
            return n;

        if (dp[k][n] != null)
            return dp[k][n];

        int left = 1;
        int right = n;
        int ans = Integer.MAX_VALUE;

        while (left <= right) {

            int mid = left + (right - left) / 2;

            int broken = superEggDrop(k - 1, mid - 1);
            int survive = superEggDrop(k, n - mid);

            ans = Math.min(ans, 1 + Math.max(broken, survive));

            if (broken < survive)
                left = mid + 1;
            else
                right = mid - 1;
        }

        return dp[k][n] = ans;
    }
}
```

### Complexity

-   Time: **O(k × n log n)**
-   Space: **O(k × n)**

------------------------------------------------------------------------

# Solution 3 - Most Optimized (Moves DP)

Instead of asking:

> Minimum moves needed for `n` floors?

Ask:

> With `m` moves and `k` eggs, how many floors can be tested?

Transition:

    dp[m][k] =
    1 +
    dp[m-1][k-1] +
    dp[m-1][k]

Explanation:

-   `dp[m-1][k-1]` → egg breaks
-   `dp[m-1][k]` → egg survives
-   `+1` → current floor

Increase moves until

    dp[m][k] >= n

``` java
class Solution {

    public int superEggDrop(int k, int n) {

        int[][] dp = new int[n + 1][k + 1];

        int moves = 0;

        while (dp[moves][k] < n) {

            moves++;

            for (int egg = 1; egg <= k; egg++) {

                dp[moves][egg] =
                        1 +
                        dp[moves - 1][egg - 1] +
                        dp[moves - 1][egg];
            }
        }

        return moves;
    }
}
```

### Complexity

-   Time: **O(k × moves)** (approximately **O(k log n)**)
-   Space: **O(k × moves)**

------------------------------------------------------------------------

# Summary

  Approach             Time                        Space
  -------------------- --------------------------- --------------
  Recursive            Exponential                 Exponential
  Memoization          O(k × n²)                   O(k × n)
  DP + Binary Search   O(k × n log n)              O(k × n)
  Moves DP (Best)      O(k × moves) ≈ O(k log n)   O(k × moves)

------------------------------------------------------------------------

# Interview Takeaways

-   Binary Search is **not** the strategy used during the actual
    egg-dropping experiment.
-   Binary Search is only an optimization to find the optimal first
    floor in the DP recurrence.
-   The Moves DP solution is considered the optimal approach for this
    problem.
