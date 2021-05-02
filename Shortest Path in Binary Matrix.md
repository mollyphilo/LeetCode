# Shortest Path in Binary Matrix

In an N by N square grid, each cell is either empty (0) or blocked (1).

A _clear path from top-left to bottom-right_ has length  `k`  if and only if it is composed of cells  `C_1, C_2, ..., C_k` such that:

-   Adjacent cells  `C_i`  and  `C_{i+1}`  are connected 8-directionally (ie., they are different and share an edge or corner)
-   `C_1`  is at location  `(0, 0)`  (ie. has value  `grid[0][0]`)
-   `C_k` is at location  `(N-1, N-1)`  (ie. has value  `grid[N-1][N-1]`)
-   If  `C_i`  is located at `(r, c)`, then  `grid[r][c]`  is empty (ie. `grid[r][c] == 0`).

Return the length of the shortest such clear path from top-left to bottom-right. If such a path does not exist, return -1.

**Example 1:**

    Input: [[0,1],[1,0]]
![](https://assets.leetcode.com/uploads/2019/08/04/example1_1.png) 

    Output: 2

![](https://assets.leetcode.com/uploads/2019/08/04/example1_2.png)

**Example 2:**

    Input: [[0,0,0],[1,1,0],[1,1,0]]
![](https://assets.leetcode.com/uploads/2019/08/04/example2_1.png) 

    Output: 4
![](https://assets.leetcode.com/uploads/2019/08/04/example2_2.png)

**Note:**

1.  `1 <= grid.length == grid[0].length <= 100`
2.  `grid[r][c]`  is  `0`  or  `1`

**Solution:**

```go
func shortestPathBinaryMatrix(grid [][]int) int {
    n := len(grid)
    if n == 0 { return 0 }
    
    if grid[0][0] == 1 || grid[n-1][n-1] == 1 { return -1 }
    
    queue := [][]int{[]int{0,0}}
    steps := 0
    
    for len(queue) > 0 {
        size := len(queue)
        steps += 1
        for s := 0; s < size; s++ {
            cell := queue[0]
            r,c := cell[0],cell[1]
            queue[0] = nil 
            queue = queue[1:]

            if r == n-1 && c == n-1 { return steps }

            for _,i := range []int{r-1,r,r+1} {
                for _,j := range []int{c-1,c,c+1} {
                    if i >= 0 && i < n && j >= 0 && j < n && !(i == r && j == c) && grid[i][j] == 0 {
                        queue = append(queue, []int{i,j})
                        grid[i][j] = -1
                    }
                }
            }
        }
    }
    
    return -1
}
```

Time complexity: O(N^2) because we visit as most N^2 cells

Space complexity: O(N^2) for the queue, which contains at most N^2 cells