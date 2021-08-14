# Set Matrix Zeroes

Given an  `m x n`  integer matrix  `matrix`, if an element is  `0`, set its entire row and column to  `0`'s, and return  _the matrix_.

You must do it  [in place](https://en.wikipedia.org/wiki/In-place_algorithm).

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/17/mat1.jpg)

	Input: matrix = [[1,1,1],[1,0,1],[1,1,1]]
	Output: [[1,0,1],[0,0,0],[1,0,1]]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/08/17/mat2.jpg)

	Input: matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
	Output: [[0,0,0,0],[0,4,5,0],[0,3,1,0]]

**Constraints:**

-   `m == matrix.length`
-   `n == matrix[0].length`
-   `1 <= m, n <= 200`
-   `-231  <= matrix[i][j] <= 231  - 1`

**Follow up:**

-   A straightforward solution using  `O(mn)`  space is probably a bad idea.
-   A simple improvement uses  `O(m + n)`  space, but still not the best solution.
-   Could you devise a constant space solution?

**Solution:**

```go
func setZeroes(matrix [][]int)  {
  m,n := len(matrix),len(matrix[0])
  
  var firstCol bool
  
  for i := 0; i < m; i++ {
    if matrix[i][0] == 0 {
      firstCol = true    
    }
    for j := 1; j < n; j++ {
      if matrix[i][j] == 0 {
        matrix[i][0] = 0
        matrix[0][j] = 0
      }
    }
  }
  
  for i := 1; i < m; i++ {
    for j := 1; j < n; j++ {
      if matrix[0][j] == 0 || matrix[i][0] == 0 {
        matrix[i][j] = 0
      }
    }
  }
  
  if matrix[0][0] == 0 { 
    for j := 0; j < n; j++ { matrix[0][j] = 0 }
  }
  
  if firstCol {
    for i := 0; i < m; i++ { matrix[i][0]  = 0 }    
  }
}
```