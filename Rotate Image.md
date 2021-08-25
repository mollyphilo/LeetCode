# Rotate Image 

You are given an  _n_  x  _n_  2D  `matrix`  representing an image, rotate the image by 90 degrees (clockwise).

You have to rotate the image  [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm), which means you have to modify the input 2D matrix directly.  **DO NOT**  allocate another 2D matrix and do the rotation.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/28/mat1.jpg)

    Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
    Output: [[7,4,1],[8,5,2],[9,6,3]]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/08/28/mat2.jpg)

    Input: matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
    Output: [[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]

**Example 3:**

    Input: matrix = [[1]]
    Output: [[1]]

**Example 4:**

    Input: matrix = [[1,2],[3,4]]
    Output: [[3,1],[4,2]]

**Constraints:**

-   `matrix.length == n`
-   `matrix[i].length == n`
-   `1 <= n <= 20`
-   `-1000 <= matrix[i][j] <= 1000`

---

**Solution:**

Transpose (reflect diagonally) then reflect horizontally

```go
func rotate(matrix [][]int)  {
    n := len(matrix)
    
    // transpose
    for i := 0; i < n; i++ {
        for j := i+1; j < n; j++ {
            matrix[i][j],matrix[j][i] = matrix[j][i],matrix[i][j]
        }
    }
    // reflect
    for i := 0; i < n; i++ {
        for j := 0; j < n/2; j++ {
            matrix[i][j],matrix[i][n-1-j] = matrix[i][n-1-j],matrix[i][j]
        }
    }
}
```

If we reflect then transpose, our matrix will rotate counter-clockwise

Approach 2: Rotate group of 4 cells

```go
func rotate(matrix [][]int)  {
  n := len(matrix)
  for i := 0; i < n/2 + n%2; i++ {
    for j := 0; j < n/2; j++ {
      temp := matrix[n-1-j][i]
      matrix[n-1-j][i] = matrix[n-1-i][n-1-j]
      matrix[n-1-i][n-1-j] = matrix[j][n-1-i]
      matrix[j][n-1-i] = matrix[i][j]
      matrix[i][j] = temp
    }
  }
}
```