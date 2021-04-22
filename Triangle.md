# Triangle
Given a  `triangle`  array, return  _the minimum path sum from top to bottom_.

For each step, you may move to an adjacent number of the row below. More formally, if you are on index  `i`  on the current row, you may move to either index  `i`  or index  `i + 1`  on the next row.

**Example 1:**

    Input: triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]
    Output: 11
    Explanation: The triangle looks like:
        2
       3 4
      6 5 7
     4 1 8 3
    The minimum path sum from top to bottom is 2 + 3 + 5 + 1 = 11 (underlined above).

**Example 2:**

Input: triangle = [[-10]]
Output: -10

**Constraints:**

-   `1 <= triangle.length <= 200`
-   `triangle[0].length == 1`
-   `triangle[i].length == triangle[i - 1].length + 1`
-   `-104  <= triangle[i][j] <= 104`

**Follow up:** Could you do this using only `O(n)` extra space, where `n` is the total number of rows in the triangle?

---

**Solution:**

```go
func minimumTotal(triangle [][]int) int {
    n := len(triangle)
    if n == 0 { return 0 }
    if n == 1 { return triangle[0][0] }
    dp := make([][]int, n)
    dp[0] = triangle[0]

    var m int
    for i := 1; i < n-1; i++ {
        dp[i] = triangle[i]
        dp[i][0] += dp[i-1][0]
        fmt.Println(dp[i])
        m = len(triangle[i])
        for j := 1; j < m-1; j++ {
            dp[i][j] += min(dp[i-1][j],dp[i-1][j-1])
        }
        dp[i][m-1] += dp[i-1][m-2]
    }
    
    dp[n-1] = triangle[n-1]
    dp[n-1][0] += dp[n-2][0]
    ans := dp[n-1][0]
    m = len(triangle[n-1])
    for j := 1; j < m-1; j++ {
        dp[n-1][j] += min(dp[n-2][j],dp[n-2][j-1])
        ans = min(ans,dp[n-1][j])
    }
    dp[n-1][m-1] += dp[n-2][m-2]
    ans = min(ans,dp[n-1][m-1])
    return ans
}

func min(a,b int) int {
    if a < b { return a }
    return b
}
```