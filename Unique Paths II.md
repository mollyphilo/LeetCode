# Unique Paths II

A robot is located at the top-left corner of a  `m x n`  grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and space is marked as  `1`  and  `0`  respectively in the grid.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/04/robot1.jpg)

**Input:** obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
**Output:** 2
**Explanation:** There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:
1. Right -> Right -> Down -> Down
2. Down -> Down -> Right -> Right

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/04/robot2.jpg)

**Input:** obstacleGrid = [[0,1],[0,0]]
**Output:** 1

**Constraints:**

-   `m == obstacleGrid.length`
-   `n == obstacleGrid[i].length`
-   `1 <= m, n <= 100`
-   `obstacleGrid[i][j]`  is  `0`  or  `1`.

**Solution:**

```go
func uniquePathsWithObstacles(obstacleGrid [][]int) int {
    n := len(obstacleGrid)
    m := len(obstacleGrid[0])
    if obstacleGrid[n-1][m-1] == 1 {
        return 0
    }
    obstacleGrid[n-1][m-1] = 1
    
    for i := n-2; i >= 0; i-- {
        if obstacleGrid[i][m-1] == 1 {
            obstacleGrid[i][m-1] = 0
        }else {
            obstacleGrid[i][m-1] = obstacleGrid[i+1][m-1]
        }
    }
    
    for i := m-2; i >= 0; i-- {
        if obstacleGrid[n-1][i] == 1 {
            obstacleGrid[n-1][i] = 0
        }else {
            obstacleGrid[n-1][i] = obstacleGrid[n-1][i+1]
        }
    }
    
    for i := n-2; i >= 0; i-- {
        for j := m-2; j >= 0; j-- {
            if obstacleGrid[i][j] == 1 {
                obstacleGrid[i][j] = 0
            }else {
                obstacleGrid[i][j] = obstacleGrid[i][j+1] + obstacleGrid[i+1][j]
            }
        }
    }
    
    return obstacleGrid[0][0]
}
```