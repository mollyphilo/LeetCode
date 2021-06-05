# Maximum Performance of a Team

You are given two integers  `n`  and  `k`  and two integer arrays  `speed`  and  `efficiency`  both of length  `n`. There are  `n`  engineers numbered from  `1`  to  `n`.  `speed[i]`  and  `efficiency[i]`  represent the speed and efficiency of the  `ith`  engineer respectively.

Choose  **at most**  `k`  different engineers out of the  `n`  engineers to form a team with the maximum  **performance**.

The performance of a team is the sum of their engineers' speeds multiplied by the minimum efficiency among their engineers.

Return  _the maximum performance of this team_. Since the answer can be a huge number, return it  **modulo**  `109  + 7`.

**Example 1:**

Input: n = 6, speed = [2,10,3,1,5,8], efficiency = [5,4,3,9,7,2], k = 2
Output: 60
Explanation: 
We have the maximum performance of the team by selecting engineer 2 (with speed=10 and efficiency=4) and engineer 5 (with speed=5 and efficiency=7). That is, performance = (10 + 5) * min(4, 7) = 60.

**Example 2:**

Input: n = 6, speed = [2,10,3,1,5,8], efficiency = [5,4,3,9,7,2], k = 3
Output: 68
Explanation: This is the same example as the first but k = 3. We can select engineer 1, engineer 2 and engineer 5 to get the maximum performance of the team. That is, performance = (2 + 10 + 5) * min(5, 4, 7) = 68.

**Example 3:**

Input: n = 6, speed = [2,10,3,1,5,8], efficiency = [5,4,3,9,7,2], k = 4
Output: 72

**Constraints:**

-   `1 <= <= k <= n <= 105`
-   `speed.length == n`
-   `efficiency.length == n`
-   `1 <= speed[i] <= 105`
-   `1 <= efficiency[i] <= 108`

**Solution:**

```go
import (
    "container/heap"
    "sort"
)

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

func maxPerformance(n int, speed []int, efficiency []int, k int) int {
    var max,e,s,sum int
    h := &MinHeap{}
    heap.Init(h)

    // map efficiency with speed
    e2s := make([][2]int,n)
    for i := 0; i < n; i++ {
        e2s[i] = [2]int{efficiency[i],speed[i]}
    }
    // sort by efficiency
    sort.Slice(e2s, func (i,j int) bool { return e2s[i][0] > e2s[j][0] })

    // interate through e2s pairs
    for i := 0; i < n; i++ {
        e = e2s[i][0]
        s = e2s[i][1]
        if len(*h) > k-1 {
            v := heap.Pop(h)
            sum -= v.(int)
        }
        heap.Push(h,s)
        sum += s
        if v := sum*e; v > max {
            max = v
        }
    }
    
    return max % 1000000007
}
```