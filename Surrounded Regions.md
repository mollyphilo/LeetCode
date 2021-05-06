# Surrounded Regions
Given an  `m x n`  matrix  `board`  containing  `'X'`  and  `'O'`,  _capture all regions surrounded by_  `'X'`.

A region is  **captured**  by flipping all  `'O'`s into  `'X'`s in that surrounded region.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/xogrid.jpg)

    Input: board = [["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]
    Output: [["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]
    Explanation: Surrounded regions should not be on the border, which means that any 'O' on the border of the board are not flipped to 'X'. Any 'O' that is not on the border and it is not connected to an 'O' on the border will be flipped to 'X'. Two cells are connected if they are adjacent cells connected horizontally or vertically.

**Example 2:**

    Input: board = [["X"]]
    Output: [["X"]]

**Constraints:**

-   `m == board.length`
-   `n == board[i].length`
-   `1 <= m, n <= 200`
-   `board[i][j]`  is  `'X'`  or  `'O'`.

**Solution:**

```go
func solve(board [][]byte)  {
    n := len(board)
    if n <= 2 { return }
    m := len(board[0])
    
    for i := 0; i < n; i++ {
        for j := 0; j < m; j++ {
            if board[i][j] == 'O' && (i == 0 || i == n-1 || j == 0 || j == m-1) {
                fill(board, i,j)
            }
        }
    }
    
    for i := 0; i < len(board); i++ {
        for j := 0; j < len(board[0]); j++ {
            if board[i][j] == 'S' {
                board[i][j] = 'O'
            }else if board[i][j] == 'O' {
                board[i][j] = 'X'
            }
        }
    }
}

func fill(board [][]byte, i,j int) {
    board[i][j] = 'S'
    if i-1 >= 0 && board[i-1][j] == 'O' {
        fill(board, i-1,j)
    }
    if i+1 < len(board) && board[i+1][j] == 'O' {
        fill(board, i+1,j)
    }
    if j-1 >= 0 && board[i][j-1] == 'O' {
        fill(board, i,j-1)
    }
    if j+1 < len(board[0]) && board[i][j+1] == 'O' {
        fill(board, i,j+1)
    }
}
```