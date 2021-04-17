# Number of Submatrices That Sum to Target

Given a  `matrix` and a  `target`, return the number of non-empty submatrices that sum to  target.

A submatrix  `x1, y1, x2, y2`  is the set of all cells  `matrix[x][y]`  with  `x1 <= x <= x2`  and  `y1 <= y <= y2`.

Two submatrices  `(x1, y1, x2, y2)`  and  `(x1', y1', x2', y2')`  are different if they have some coordinate that is different: for example, if  `x1 != x1'`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/02/mate1.jpg)

    Input: matrix = [[0,1,0],[1,1,1],[0,1,0]], target = 0
    Output: 4
    Explanation: The four 1x1 submatrices that only contain 0.

**Example 2:**

    Input: matrix = [[1,-1],[-1,1]], target = 0
    Output: 5
    Explanation: The two 1x2 submatrices, plus the two 2x1 submatrices, plus the 2x2 submatrix.

**Example 3:**

    Input: matrix = [[904]], target = 0
    Output: 0

**Constraints:**

-   `1 <= matrix.length <= 100`
-   `1 <= matrix[0].length <= 100`
-   `-1000 <= matrix[i] <= 1000`
-   `-10^8 <= target <= 10^8`

---

**Solution:**

```go
func numSubmatrixSumTarget(matrix [][]int, target int) int {
    rows,cols := len(matrix),len(matrix[0])

    var tb map[int]int
    var colsums []int
    var rowsum,ans int
    for r1 := 0; r1 < rows; r1++ {
        colsums = make([]int, cols)
        for r2 := r1; r2 < rows; r2++ {
            tb = map[int]int{0: 1}
            rowsum = 0
            for c := 0; c < cols; c++ {
                rowsum += matrix[r2][c] # range sum query by row
                colsums[c] += rowsum # rang sum query by column
                ans += tb[colsums[c]-target] # subarray sum equals target
                tb[colsums[c]] += 1
            }
        }
    }
    
    return ans
}
```