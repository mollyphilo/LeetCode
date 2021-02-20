# Perfect Squares
Given an integer  `n`, return  _the least number of perfect square numbers that sum to_  `n`.

A  **perfect square**  is an integer that is the square of an integer; in other words, it is the product of some integer with itself. For example,  `1`,  `4`,  `9`, and  `16`  are perfect squares while  `3`  and  `11`  are not.

**Example 1:**

	Input: n = 12
	Output: 3
	Explanation: 12 = 4 + 4 + 4.

**Example 2:**

	Input: n = 13
	Output: 2
	Explanation: 13 = 4 + 9.

**Constraints:**

-   `1 <= n <= 104`

**Solution:**

```go
import "math"

func numSquares(n int) int {
    max := int(math.Sqrt(float64(n)))
    dp := make([]int,n+1)
    
    for i := 0; i <=n; i++ {
        dp[i] = n+1
    }
    var sqrt int
    for i := 1; i <= n; i++ {
        for j := 1; j <= max; j++ {
            sqrt = j*j
            if i == sqrt {
                dp[i] = 1
            }else if i > sqrt && dp[i] > dp[i-sqrt]+1{
                dp[i] = dp[i - sqrt]+1
            }
        }
    }
    
    return dp[n]
}
```