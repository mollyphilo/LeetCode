# Spiral Matrix

Given an  `m x n`  `matrix`, return  _all elements of the_  `matrix`  _in spiral order_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/13/spiral1.jpg)

	Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
	Output: [1,2,3,6,9,8,7,4,5]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/13/spiral.jpg)

	Input: matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
	Output: [1,2,3,4,8,12,11,10,9,5,6,7]

**Constraints:**

-   `m == matrix.length`
-   `n == matrix[i].length`
-   `1 <= m, n <= 10`
-   `-100 <= matrix[i][j] <= 100`

**Solution:**

```go
func spiralOrder(matrix [][]int) []int {
  m,n := len(matrix),len(matrix[0])
  layers := (min(m,n) + 1)/2
  ans := make([]int, m*n)
  var ai int
  
  for l := 0; l < layers; l++ {
    if l == n-1-l { 
      for i := l; i < m-l; i++ { ans[ai] = matrix[i][n-1-l]; ai++ }
      continue
    }
    if l == m-1-l {
      for i := l; i < n-l; i++ { ans[ai] = matrix[l][i]; ai++ }
      continue
    }
    for i := l; i < n-1-l; i++ { ans[ai] = matrix[l][i]; ai++ }
    for i := l; i < m-1-l; i++ { ans[ai] = matrix[i][n-1-l]; ai++ }
    for i := n-1-l; i > l; i-- { ans[ai] = matrix[m-1-l][i]; ai++ }
    for i := m-1-l; i > l; i-- { ans[ai] = matrix[i][l]; ai++ }
  }
  
  return ans
}

func min(a,b int) int {
  if a < b { return a }
  return b
}
```