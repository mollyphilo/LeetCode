# Rectangle Area II

We are given a list of (axis-aligned)  `rectangles`. Each  `rectangle[i] = [xi1, yi1, xi2, yi2]` , where  `(xi1, yi1)`  are the coordinates of the bottom-left corner, and  `(xi2, yi2)`  are the coordinates of the top-right corner of the  `ith`  rectangle.

Find the total area covered by all  `rectangles`  in the plane. Since the answer may be too large, return it  **modulo**  `109  + 7`.

**Example 1:**

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/06/06/rectangle_area_ii_pic.png)

	Input: rectangles = [[0,0,2,2],[1,0,2,3],[1,0,3,1]]
	Output: 6
	Explanation: As illustrated in the picture.

**Example 2:**

	Input: rectangles = [[0,0,1000000000,1000000000]]
	Output: 49
	Explanation: The answer is 1018 modulo (109 + 7), which is (109)2 = (-7)2 = 49.

**Constraints:**

-   `1 <= rectangles.length <= 200`
-   `rectanges[i].length = 4`
-   `0 <= rectangles[i][j] <= 109`
-   The total area covered by all rectangles will never exceed  `263  - 1`  and thus will fit in a  **64-bit**  signed integer.

**Solution:**

R = size of rectangle, Nx = #unique x, Ny = #unique y

Time Complexity: O(R * (Nx + Ny)) + O(Nx + Ny) + O(R * Nx * Ny)  =  O(R * Nx * Ny) 

Space Complexity: O(Nx + Ny) + O(Nx * Ny) = O(Nx * Ny)

```go
import "sort"

func rectangleArea(rectangles [][]int) int {
  // find and sort all distint values of x and y
  xmap,ymap := make(map[int]int),make(map[int]int)
  var xval,yval []int
  for _,rec := range rectangles {
    if xmap[rec[0]] == 0 {
      xmap[rec[0]] = 1
      xval = append(xval, rec[0])
    }
    if xmap[rec[2]] == 0 {
      xmap[rec[2]] = 1
      xval = append(xval, rec[2])
    }
    if ymap[rec[1]] == 0 {
      ymap[rec[1]] = 1
      yval = append(yval, rec[1])
    }
    if ymap[rec[3]] == 0 {
      ymap[rec[3]] = 1
      yval = append(yval, rec[3])
    }
  }
  sort.Ints(xval)
  sort.Ints(yval)
  
  // map x and y to their sorted indexes
  for i := 0; i < len(xval); i++ {
    xmap[xval[i]] = i // 0 1 2 3
  }
  for i := 0; i < len(yval); i++ {
    ymap[yval[i]] = i // 0 1 2 3
  }
  // mark cell is filled if its x and y found from rectangles
  grid := make([][]bool, len(xval))
  for i := 0; i < len(xval); i++ {
    grid[i] = make([]bool, len(yval))
  }
  for _,rec := range rectangles {
    for x := xmap[rec[0]]; x < xmap[rec[2]]; x++ { // 0 -> 2
      for y := ymap[rec[1]]; y < ymap[rec[3]]; y++ { // 0 -> 2
        grid[x][y] = true
      }
    }
  }
  // map back index to its x or y value
  var ans int
  for x := 0; x < len(xval); x++ {
    for y := 0; y < len(yval); y++ {
      if grid[x][y] {
        ans += (xval[x+1]-xval[x])*(yval[y+1]-yval[y])
      }
    }
  }
  
  return ans % (int(1e9)+7)
}
```