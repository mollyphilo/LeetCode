# Integer Break

Given a positive integer  _n_, break it into the sum of  **at least**  two positive integers and maximize the product of those integers. Return the maximum product you can get.

**Example 1:**

**Input:** 2
**Output:** 1
**Explanation:** 2 = 1 + 1, 1 × 1 = 1.

**Example 2:**

**Input:** 10
**Output:** 36
**Explanation:** 10 = 3 + 3 + 4, 3 × 3 × 4 = 36.

**Note**: You may assume that  _n_  is not less than 2 and not larger than 58.

**Solution:**

```go
func integerBreak(n int) int {
    if n == 2 { return 1 }
    dp := make([]int, n+1)
    dp[2] = 1
    
    for i := 3; i <= n ; i++ {
        for j := 2; j < i; j++ {
            dp[i] = max(dp[i],j * max(i-j,dp[i-j]))
        } 
    }
    
    return dp[n]
}

func max(a,b int) int {
    if a > b { return a }
    return b
}
```