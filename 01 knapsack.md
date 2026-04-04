🔁 1. Recursive Solution (Brute Force)
💡 Idea

For every item, we have two choices:

Include it
Exclude it
⏱ Complexity
Time: O(2ⁿ)
Space: O(n) (recursion stack)
class Solution {
    public int knapsack(int W, int val[], int wt[]) {
        int n = wt.length;
        return helper(W, val, wt, n);
    }

    public int helper(int W, int val[], int wt[], int n) {
        
        // Base case
        if (W == 0 || n == 0) {
            return 0;
        }

        int currentWeight = wt[n - 1];
        int currentValue = val[n - 1];

        // If weight exceeds capacity → cannot include
        if (currentWeight > W) {
            return helper(W, val, wt, n - 1);
        } 
        else {
            // Include the item
            int include = currentValue + helper(W - currentWeight, val, wt, n - 1);

            // Exclude the item
            int exclude = helper(W, val, wt, n - 1);

            // Return max of both choices
            return Math.max(include, exclude);
        }
    }
}
🧠 2. Memoization (Top-Down DP)
💡 Idea

Store results of subproblems to avoid recomputation.

⏱ Complexity
Time: O(n × W)
Space: O(n × W) + recursion stack
class Solution {
    public int knapsack(int w, int val[], int wt[]) {
        
        int n = wt.length;
        int[][] dp= new int[n+1][w+1];
        
        // Initialize with -1
        for(int i=0 ; i<=n; i++){
            for(int j=0 ; j<=w ; j++){
                dp[i][j]=-1;
            }
        }

        return helper(w, val, wt, n, dp);
    }
    
    public int helper(int w, int val[], int wt[], int n, int dp[][]) {
        if (w == 0 || n == 0) {
            return 0;
        }
        
        // Check if already computed
        if(dp[n][w] != -1){
            return dp[n][w];
        }
        
        int currentweight = wt[n - 1];
        int currentvalue = val[n - 1];
        
        if (currentweight > w) {
            dp[n][w]= helper(w, val, wt, n - 1, dp);
        }
        else{
            int include = currentvalue + helper(w - currentweight, val, wt, n - 1, dp);
            int exclude = helper(w, val, wt, n - 1, dp);
            
            dp[n][w]= Math.max(include, exclude);
        }
        
        return dp[n][w];
    }
}
📊 3. Bottom-Up (Tabulation)
💡 Idea

Build solution iteratively using a DP table.

⏱ Complexity
Time: O(n × W)
Space: O(n × W)
class Solution {
    public int knapsack(int W, int val[], int wt[]) {
        int n = wt.length;

        // dp[i][w] = max value using first i items with capacity w
        int[][] dp = new int[n + 1][W + 1];

        // Build the table
        for (int i = 1; i <= n; i++) {
            for (int w = 1; w <= W; w++) {

                // Cannot include item
                if (wt[i - 1] > w) {
                    dp[i][w] = dp[i - 1][w];
                } 
                else {
                    int include = val[i - 1] + dp[i - 1][w - wt[i - 1]];
                    int exclude = dp[i - 1][w];

                    dp[i][w] = Math.max(include, exclude);
                }
            }
        }

        return dp[n][W];
    }
}
🔥 Key Takeaways
Approach	Time Complexity	Space Complexity	Notes
Recursive	O(2ⁿ)	O(n)	Simple but slow
Memoization	O(n×W)	O(n×W)	Optimized recursion
Tabulation	O(n×W)	O(n×W)	Most efficient
🚀 Optimization Tip

You can further optimize space to O(W) using a 1D DP array.
