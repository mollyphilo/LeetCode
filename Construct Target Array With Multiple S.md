# Construct Target Array With Multiple Sums
Given an array of integers `target`. From a starting array,  `A` consisting of all 1's, you may perform the following procedure :

-   let  `x`  be the sum of all elements currently in your array.
-   choose index  `i`, such that `0 <= i < target.size`  and set the value of  `A`  at index  `i`  to  `x`.
-   You may repeat this procedure as many times as needed.

Return True if it is possible to construct the  `target`  array from  `A`  otherwise return False.

**Example 1:**

	Input: target = [9,3,5]
	Output: true
	Explanation: Start with [1, 1, 1] 
	[1, 1, 1], sum = 3 choose index 1
	[1, 3, 1], sum = 5 choose index 2
	[1, 3, 5], sum = 9 choose index 0
	[9, 3, 5] Done

**Example 2:**

	Input: target = [1,1,1,2]
	Output: false
	Explanation: Impossible to create target array from [1,1,1,1].

**Example 3:**

Input: target = [8,5]
Output: true

**Constraints:**

-   `N == target.length`
-   `1 <= target.length <= 5 * 10^4`
-   `1 <= target[i] <= 10^9`

**Solution:**

```go
import "container/heap"

type MaxHeap []int
func (h MaxHeap) Len() int { return len(h) }
func (h MaxHeap) Less(i,j int) bool { return h[i] > h[j] }
func (h MaxHeap) Swap(i,j int) { h[i],h[j] = h[j],h[i] }
func (h *MaxHeap) Push(x interface{}) { *h = append(*h, x.(int)) }
func (h *MaxHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0:n-1]
    return x
}

func isPossible(target []int) bool {
    n := len(target)
    h := MaxHeap{}
    heap.Init(&h)
    
    var sum int
    
    for i := 0; i < n; i++ {
        heap.Push(&h,target[i])        
        sum += target[i]
    }
    
    var t int
    for {
        t = heap.Pop(&h).(int)
        sum -= t
        if t == 1 || sum == 1 { return true } // last value in heap is 1
        if t < sum || sum == 0 || t%sum == 0 { return false } // sum should be less than t because sum + 1*x = t
        t %= sum
        heap.Push(&h,t)
        sum += t
    }
}
```