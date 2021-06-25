# Out of Boundary Paths
There is an  `m x n`  grid with a ball. The ball is initially at the position  `[startRow, startColumn]`. You are allowed to move the ball to one of the four adjacent cells in the grid (possibly out of the grid crossing the grid boundary). You can apply  **at most**  `maxMove`  moves to the ball.

Given the five integers  `m`,  `n`,  `maxMove`,  `startRow`,  `startColumn`, return the number of paths to move the ball out of the grid boundary. Since the answer can be very large, return it  **modulo**  `109  + 7`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/28/out_of_boundary_paths_1.png)

	Input: m = 2, n = 2, maxMove = 2, startRow = 0, startColumn = 0
	Output: 6

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/04/28/out_of_boundary_paths_2.png)

	Input: m = 1, n = 3, maxMove = 3, startRow = 0, startColumn = 1
	Output: 12

**Constraints:**

-   `1 <= m, n <= 50`
-   `0 <= maxMove <= 50`
-   `0 <= startRow < m`
-   `0 <= startColumn < n`

**Solution:**

Recursive with memorization

```go
const mod int = 1000000007

func findPaths(m int, n int, maxMove int, startRow int, startColumn int) int {
    memo := make([][][]int,m)
    for i := 0; i < m; i++ {
        memo[i] = make([][]int,n)
        for j := 0; j < n; j++ {
            memo[i][j] = make([]int,maxMove + 1)
            for k := 0; k <= maxMove; k++ {
                memo[i][j][k] = -1
            }
        }
        
    }
    return findpaths(m,n,maxMove,startRow,startColumn,memo)
}

func findpaths(m,n,N,i,j int, memo [][][]int) int {
    if i == m || j == n || i < 0 || j < 0 { return 1 }
    if N == 0 { return 0 }
    if memo[i][j][N] >= 0 { return memo[i][j][N] }
    memo[i][j][N] = (findpaths(m,n,N-1,i-1,j,memo) % mod + findpaths(m,n,N-1,i+1,j,memo) % mod +
                    findpaths(m,n,N-1,i,j-1,memo) % mod + findpaths(m,n,N-1,i,j+1,memo) % mod) % mod
    return memo[i][j][N]
}
```