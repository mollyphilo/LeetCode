# Longest Increasing Path in a Matrix

Given an  `m x n`  integers  `matrix`, return  _the length of the longest increasing path in_ `matrix`.

From each cell, you can either move in four directions: left, right, up, or down. You  **may not**  move  **diagonally**  or move  **outside the boundary**  (i.e., wrap-around is not allowed).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/05/grid1.jpg)

    Input: matrix = [[9,9,4],[6,6,8],[2,1,1]]
    Output: 4
    Explanation: The longest increasing path is `[1, 2, 6, 9]`.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/27/tmp-grid.jpg)

    Input: matrix = [[3,4,5],[3,2,6],[2,2,1]]
    Output: 4
    Explanation: The longest increasing path is `[3, 4, 5, 6]`. Moving diagonally is not allowed.

**Example 3:**

**Input:** matrix = [[1]]
**Output:** 1

**Constraints:**

-   `m == matrix.length`
-   `n == matrix[i].length`
-   `1 <= m, n <= 200`
-   `0 <= matrix[i][j] <= 231  - 1`

---

**Solution:**

DFS from every number in the matrix

```go
func longestIncreasingPath(matrix [][]int) int {
    n,m,max := len(matrix),len(matrix[0]),1
    dp := make([][]int, n)
    for i := 0; i < n; i++ {
        dp[i] = make([]int,m)
    }
    
    for i := 0; i < n; i++ {
        for j := 0; j < m; j++ {
            max = greater(max, walk(matrix,dp,i,j))
        }
    }
    return max
}


func walk(matrix,dp [][]int, i,j int) int{
    if dp[i][j] != 0 {
        return dp[i][j]
    }
    n,m,max := len(matrix),len(matrix[0]),1
    
    for _,idx := range [][2]int{{i-1,j}, {i+1,j}, {i, j-1}, {i,j+1}} {
        if idx[0] >= 0 && idx[0] < n && idx[1] >= 0 && idx[1] < m && matrix[idx[0]][idx[1]] > matrix[i][j] {
            max = greater(max, 1 + walk(matrix,dp,idx[0],idx[1]))
        }
    }    
    
    dp[i][j] = max
    
    return max
}

func greater(a,b int) int {
    if a > b {
        return a
    }
    return b
}
```