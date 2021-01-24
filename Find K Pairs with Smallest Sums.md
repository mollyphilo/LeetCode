# Find K Pairs with Smallest Sums
You are given two integer arrays  **nums1**  and  **nums2**  sorted in ascending order and an integer  **k**.

Define a pair  **(u,v)**  which consists of one element from the first array and one element from the second array.

Find the k pairs  **(u1,v1),(u2,v2) ...(uk,vk)**  with the smallest sums.

**Example 1:**

	Input: nums1 = [1,7,11], nums2 = [2,4,6], k = 3
	Output: [[1,2],[1,4],[1,6]] 
	Explanation: The first 3 pairs are returned from the sequence: 
	             [1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]

**Example 2:**

	Input: nums1 = [1,1,2], nums2 = [1,2,3], k = 2
	Output: [1,1],[1,1] 
	Explanation: The first 2 pairs are returned from the sequence: 
	             [1,1],[1,1],[1,2],[2,1],[1,2],[2,2],[1,3],[1,3],[2,3]

**Example 3:**

	Input: nums1 = [1,2], nums2 = [3], k = 3
	Output: [1,3],[2,3]
	Explanation: All possible pairs are returned from the sequence: [1,3],[2,3]

**Solution:**
```go
import "container/heap"

type MaxHeap [][]int

func (h MaxHeap) Len() int  { return len(h) }
func (h MaxHeap) Less(i,j int) bool { return (h[i][0] + h[i][1]) > (h[j][0] + h[j][1]) }
func (h MaxHeap) Swap(i,j int) { h[i],h[j] = h[j],h[i]}
func (h *MaxHeap) Push(x interface{}) {
    *h = append(*h, x.([]int))
}
func (h *MaxHeap) Pop() interface{} {
    if h.Len() == 0 {
        return nil
    }
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0:n-1]
    return x
}

func kSmallestPairs(nums1 []int, nums2 []int, k int) [][]int {
    h := new(MaxHeap)
    heap.Init(h)
    
    for i := 0; i < len(nums1);i++ {
        for j := 0; j < len(nums2); j++ {
            heap.Push(h, []int{nums1[i],nums2[j]})
            if h.Len() > k {
                heap.Pop(h)
            }
        }
    }
    
    heap_size := h.Len()
    if heap_size == 0 {
        return make([][]int,0)
    }
    
    result := make([][]int,heap_size)
    for i := heap_size; i > 0; i-- {
        result[i-1] = heap.Pop(h).([]int)
    }
    return result
}
```