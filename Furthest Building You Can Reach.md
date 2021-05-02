# Furthest Building You Can Reach

You are given an integer array  `heights`  representing the heights of buildings, some  `bricks`, and some  `ladders`.

You start your journey from building  `0`  and move to the next building by possibly using bricks or ladders.

While moving from building  `i`  to building  `i+1`  (**0-indexed**),

-   If the current building's height is  **greater than or equal**  to the next building's height, you do  **not**  need a ladder or bricks.
-   If the current building's height is  **less than**  the next building's height, you can either use  **one ladder**  or  `(h[i+1] - h[i])`  **bricks**.

_Return the furthest building index (0-indexed) you can reach if you use the given ladders and bricks optimally._

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/27/q4.gif)

    Input: heights = [4,2,7,6,9,14,12], bricks = 5, ladders = 1
    Output: 4
    Explanation: Starting at building 0, you can follow these steps:
    - Go to building 1 without using ladders nor bricks since 4 >= 2.
    - Go to building 2 using 5 bricks. You must use either bricks or ladders because 2 < 7.
    - Go to building 3 without using ladders nor bricks since 7 >= 6.
    - Go to building 4 using your only ladder. You must use either bricks or ladders because 6 < 9.
    It is impossible to go beyond building 4 because you do not have any more bricks or ladders.

**Example 2:**

    Input: heights = [4,12,2,7,3,18,20,3,19], bricks = 10, ladders = 2
    Output: 7

**Example 3:**

    Input: heights = [14,3,19,3], bricks = 17, ladders = 0
    Output: 3

**Constraints:**

-   `1 <= heights.length <= 105`
-   `1 <= heights[i] <= 106`
-   `0 <= bricks <= 109`
-   `0 <= ladders <= heights.length`

**Solution:**

```go
import "container/heap"

// min heap to store the height differences
// we should use ladders for the biggest differences
// if heap has more elements then the number of provided ladders, we'll need to use bricks
// therefore we can put the biggest differences at the bottom of the heap
// the pop out the smallest differences first to use with bricks as a greedy approach
type MinHeap []int
func (h MinHeap) Len() int { return len(h) }
func (h MinHeap) Less(i,j int) bool { return h[i] < h[j] }
func (h MinHeap) Swap(i,j int) { h[i],h[j] = h[j],h[i] }
func (h *MinHeap) Push(x interface{}) { *h = append(*h, x.(int)) }
func (h *MinHeap) Pop() interface{} {
  old := *h
  n := len(old)
  x := old[n-1]
  *h = old[0:n-1]
  return x
}

func furthestBuilding(heights []int, bricks int, ladders int) int {
  // initialize heap
  n := len(heights)
  h := &MinHeap{}
  heap.Init(h)
  
  var diff int
  // going through heights and push the differences to the heap
  // we will use ladders first, then bricks
  for i := 1; i < n; i++ {
    if v := heights[i]-heights[i-1]; v > 0 {
      heap.Push(h,v)

      if h.Len() > ladders {
        diff = heap.Pop(h).(int)
        if bricks < diff {
            return i-1
        }
        bricks -= diff
      }    
    }
  }
  
  return n-1
}
```