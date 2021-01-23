# Sort the Matrix Diagonally

A  **matrix diagonal**  is a diagonal line of cells starting from some cell in either the topmost row or leftmost column and going in the bottom-right direction until reaching the matrix's end. For example, the  **matrix diagonal**  starting from  `mat[2][0]`, where  `mat`  is a  `6 x 3`  matrix, includes cells  `mat[2][0]`,  `mat[3][1]`, and  `mat[4][2]`.

Given an  `m x n`  matrix  `mat`  of integers, sort each  **matrix diagonal**  in ascending order and return  _the resulting matrix_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/01/21/1482_example_1_2.png)

	Input: mat = [[3,3,1,1],[2,2,1,2],[1,1,1,2]]
	Output: [[1,1,1,1],[1,2,2,2],[1,2,3,3]]

**Constraints:**

-   `m == mat.length`
-   `n == mat[i].length`
-   `1 <= m, n <= 100`
-   `1 <= mat[i][j] <= 100`

**Solutions:**

Use array to store value for each diagonal. 
As the hint suggests, all cells in the same diagonal (i,j) have the same difference so we can get the diagonal of a cell using the difference i-j. 
Therefore, a map can be used to store diagonal array with the difference `i-j` be the key.

```go
import "sort"

func diagonalSort(mat [][]int) [][]int {
    table := make(map[int][]int)
    n,m := len(mat),len(mat[0])
    // put diagonal values into array
    for i := 0; i < n; i++ {
        for j := 0; j < m; j++ {
            val, ok := table[i-j]
            if !ok {
                val = []int{}
            }
            val = append(val, mat[i][j])
            table[i-j] = val
        }
    }
    // sort diagonal arrays
    for key := range table {
        sort.Ints(table[key])       
    }

    // put sorted value back to the matrix
    for i := 0; i < n; i++ {
        for j := 0; j < m; j++ {
            mat[i][j] = table[i-j][0]
            table[i-j] = table[i-j][1:]
        }
    }
    
    return mat
}
``` 