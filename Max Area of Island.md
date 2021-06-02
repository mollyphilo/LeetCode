
# Max Area of Island
You are given an  `m x n`  binary matrix  `grid`. An island is a group of  `1`'s (representing land) connected  **4-directionally**  (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

The  **area**  of an island is the number of cells with a value  `1`  in the island.

Return  _the maximum  **area**  of an island in_ `grid`. If there is no island, return  `0`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/05/01/maxarea1-grid.jpg)

	Input: grid = [[0,0,1,0,0,0,0,1,0,0,0,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,1,1,0,1,0,0,0,0,0,0,0,0],[0,1,0,0,1,1,0,0,1,0,1,0,0],[0,1,0,0,1,1,0,0,1,1,1,0,0],[0,0,0,0,0,0,0,0,0,0,1,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,0,0,0,0,0,0,1,1,0,0,0,0]]
	Output: 6
	Explanation: The answer is not 11, because the island must be connected 4-directionally.

**Example 2:**

	Input: grid = [[0,0,0,0,0,0,0,0]]
	Output: 0

**Constraints:**

-   `m == grid.length`
-   `n == grid[i].length`
-   `1 <= m, n <= 50`
-   `grid[i][j]`  is either  `0`  or  `1`.

**Solution:**

```go
func maxAreaOfIsland(grid [][]int) int {
    var max int
    for i := 0; i < len(grid); i++ {
        for j := 0; j < len(grid[0]); j++ {
            if grid[i][j] == 1 {
                count := 0
                dfs(&grid,i,j,&count)
                if count > max {
                    max = count
                }
            }
        }
    }
    
    return max
}

func dfs(grid *[][]int, i,j int, count *int) {
    *count++
    (*grid)[i][j] = 0
    for _,p := range [][]int{{i-1,j},{i+1,j},{i,j-1},{i,j+1}} {
        if p[0] >= 0 && p[0] < len(*grid) && p[1] >= 0 && p[1] < len((*grid)[0]) && (*grid)[p[0]][p[1]] == 1 {
            dfs(grid,p[0],p[1],count)
        }
    }
}
```