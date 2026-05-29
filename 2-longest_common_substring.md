# Longest Common Substring — Recursive & Tabulation Approaches

# Problem
Find the length of the **Longest Common Substring** between two strings.

---

# 1. Recursive Approach

## Key Idea
The recursive function tells:

> "What is the length of the common substring ending at `(i, j)`?"

If characters match:
- continue diagonally

If characters do not match:
- substring breaks
- return `0`

---

## Trick to Remember in Interview

### Subsequence
- can skip characters
- multiple recursive branches

### Substring
- must be continuous
- only diagonal movement allowed

So:

```text
match    -> go diagonal
mismatch -> reset to 0
```

---

## Recursive Code

```java
class Solution {
    
    public int longCommSubstr(String s1, String s2) {
        
        int n = s1.length();
        int m = s2.length();
        
        int ans = 0;
        
        for(int i = 0; i < n; i++) {
            for(int j = 0; j < m; j++) {
                ans = Math.max(ans, helper(s1, s2, i, j));
            }
        }
        
        return ans;
    }
    
    public int helper(String s1, String s2, int i, int j) {
        
        if(i < 0 || j < 0) {
            return 0;
        }
        
        if(s1.charAt(i) == s2.charAt(j)) {
            return 1 + helper(s1, s2, i - 1, j - 1);
        }
        
        return 0;
    }
}
```

---

## Time Complexity

```text
O(N × M × min(N, M))
```

---

## Space Complexity

```text
O(min(N, M))
```

---

## Quick Revision

```text
1. Match -> diagonal + 1
2. Mismatch -> 0
3. Try all (i, j)
4. Take global maximum
```

---

# 2. Tabulation (DP) Approach

## Key Idea

```text
dp[i][j]
= length of longest common substring
ending at s1[i-1] and s2[j-1]
```

---

## Transition

### If characters match

```text
dp[i][j] = 1 + dp[i-1][j-1]
```

### Else

```text
dp[i][j] = 0
```

---

## Trick to Remember in Interview

```text
Top      ❌
Left     ❌
Diagonal ✅
```

Only diagonal matters in substring.

---

## Tabulation Code

```java
class Solution {
    
    public int longCommSubstr(String s1, String s2) {
        
        int n = s1.length();
        int m = s2.length();
        
        int[][] dp = new int[n + 1][m + 1];
        
        int ans = 0;
        
        for(int i = 1; i <= n; i++) {
            
            for(int j = 1; j <= m; j++) {
                
                if(s1.charAt(i - 1) == s2.charAt(j - 1)) {
                    
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                    
                    ans = Math.max(ans, dp[i][j]);
                    
                } else {
                    
                    dp[i][j] = 0;
                }
            }
        }
        
        return ans;
    }
}
```

---

## Time Complexity

```text
O(N × M)
```

---

## Space Complexity

```text
O(N × M)
```

---

# Final Difference

```text
Subsequence -> can skip
Substring   -> continuous
```

