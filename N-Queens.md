# N-Queens

The  **n-queens**  puzzle is the problem of placing  `n`  queens on an  `n x n`  chessboard such that no two queens attack each other.

Given an integer  `n`, return  _all distinct solutions to the  **n-queens puzzle**_.

Each solution contains a distinct board configuration of the n-queens' placement, where  `'Q'`  and  `'.'`  both indicate a queen and an empty space, respectively.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

	Input: n = 4
	Output: [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
	Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above

**Example 2:**

	Input: n = 1
	Output: [["Q"]]

**Constraints:**

-   `1 <= n <= 9`

**Solution:**

```go
import "strings"

func solveNQueens(n int) [][]string {
    var res [][]string
    cols := make(map[int]struct{})
    mainDiag := make(map[int]struct{})
    antiDiag := make(map[int]struct{})
    backtrack(0,n,[]string{},&res,cols,mainDiag,antiDiag)
    
    return res
}

func backtrack(row,n int, sol []string, res *[][]string, cols,mainDiag,antiDiag map[int]struct{}) {
	// base case
    if row == n {
        // found a solution. Add its copy to result 
        // (if not we will have duplicated string from the append and remove step in L36 and L48)
        temp := make([]string,len(sol))
        copy(temp,sol)
        *res = append(*res,temp)
        return
    }
    
    // try to put Queen on each col on this row
    for col := 0; col < n; col++ {
        // eliminate cells that existing Queens can cross
        if _,ok := cols[col]; ok { continue }
        if _,ok := mainDiag[row-col]; ok { continue }
        if _,ok := antiDiag[row+col]; ok { continue }

        temp := make([]byte,n)
        for i := 0; i < n; i++ {
            temp[i] = '.'
        }
        temp[col] = 'Q'

        sol = append(sol,string(temp))
        cols[col] = struct{}{}
        mainDiag[row-col] = struct{}{}
        antiDiag[row+col] = struct{}{}
        
        // run backtrack for the next row
        backtrack(row+1,n,sol,res,cols,mainDiag,antiDiag)
        
        // revert changes to run backtrack for next value of col
        delete(cols,col)
        delete(mainDiag,row-col)
        delete(antiDiag,row+col)
        sol = sol[0:len(sol)-1]
    }
}
```