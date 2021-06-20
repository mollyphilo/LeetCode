# Swim in Rising Water

On an N x N  `grid`, each square  `grid[i][j]`  represents the elevation at that point  `(i,j)`.

Now rain starts to fall. At time  `t`, the depth of the water everywhere is  `t`. You can swim from a square to another 4-directionally adjacent square if and only if the elevation of both squares individually are at most `t`. You can swim infinite distance in zero time. Of course, you must stay within the boundaries of the grid during your swim.

You start at the top left square  `(0, 0)`. What is the least time until you can reach the bottom right square  `(N-1, N-1)`?

**Example 1:**

	Input: [[0,2],[1,3]]
	Output: 3
	Explanation:
	At time `0`, you are in grid location `(0, 0)`.
	You cannot go anywhere else because 4-directionally adjacent neighbors have a higher elevation than t = 0.

You cannot reach point `(1, 1)` until time `3`.
When the depth of water is `3`, we can swim anywhere inside the grid.

**Example 2:**

	Input: [[0,1,2,3,4],[24,23,22,21,5],[12,13,14,15,16],[11,17,18,19,20],[10,9,8,7,6]]
	Output: 16
	Explanation:

 **0  1  2  3  4**
24 23 22 21  **5**
**12 13 14 15 16**
**11** 17 18 19 20
**10  9  8  7  6**

The final route is marked in bold.
We need to wait until time 16 so that (0, 0) and (4, 4) are connected.

**Note:**

1.  `2 <= N <= 50`.
2.  grid[i][j] is a permutation of [0, ..., N*N - 1].

**Solution:**

```go
import "container/heap"
// Dijkstras

type Item struct {
    row,col,value int
}
type PriorityQueue []Item
func (pq PriorityQueue) Len() int { return len(pq) }
func (pq PriorityQueue) Less(i,j int) bool { return pq[i].value < pq[j].value }
func (pq PriorityQueue) Swap(i,j int) { pq[i],pq[j] = pq[j],pq[i] }
func (pq *PriorityQueue) Push(x interface{}) { *pq = append(*pq, x.(Item)) }
func (pq *PriorityQueue) Pop() interface{} {
    old := *pq
    n := len(old)
    x := old[n-1]
    *pq = old[0:n-1]
    return x
}

func swimInWater(grid [][]int) int {
    n := len(grid)
    pq := &PriorityQueue{Item{row: 0, col: 0,value: grid[0][0]}}
    heap.Init(pq)

    visited := make([][]bool,n)
    for i := 0; i < n; i++ {
        visited[i] = make([]bool,n)
    }
    
    directions := [][]int{{-1,0},{1,0},{0,-1},{0,1}}
    
    for len(*pq) > 0 {
        item := heap.Pop(pq).(Item)
        visited[item.row][item.col] = true
        if item.row == n-1 && item.col == n-1 {
            return max(item.value,grid[n-1][n-1])
        }
        
        for _,dir := range directions {
            r,c := item.row + dir[0], item.col + dir[1]
            
            if r < 0 || r == n || c < 0 || c == n || visited[r][c] { continue }
            heap.Push(pq, Item{row: r,col: c, value: max(grid[r][c],item.value)})
        }
    }
    
    return -1
}

func max(a,b int) int {
    if a > b { return a }
    return b
}
```