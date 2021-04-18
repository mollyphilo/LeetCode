# Top K Frequent Elements
Given an integer array  `nums`  and an integer  `k`, return  _the_  `k`  _most frequent elements_. You may return the answer in  **any order**.

**Example 1:**

	Input: nums = [1,1,1,2,2,3], k = 2
	Output: [1,2]

**Example 2:**

	Input: nums = [1], k = 1
	Output: [1]

**Constraints:**

-   `1 <= nums.length <= 105`
-   `k`  is in the range  `[1, the number of unique elements in the array]`.
-   It is  **guaranteed**  that the answer is  **unique**.

**Follow up:**  Your algorithm's time complexity must be better than  `O(n log n)`, where n is the array's size.

----

**Solution:**

```go
import "container/heap"

type Item struct {
    value int
    priority int // frequency
    // The index is needed by update and is maintained by the heap.Interface methods.
    index int // the index of the item in the heap
}
type PriorityQueue []*Item

func (pq PriorityQueue) Len() int { return len(pq) }
func (pq PriorityQueue) Less(i,j int) bool { return pq[i].priority > pq[j].priority }
func (pq PriorityQueue) Swap(i,j int) {
    pq[i],pq[j] = pq[j],pq[i]
    pq[i].index = i
    pq[j].index = j
}
func (pq *PriorityQueue) Push(x interface{}) {
    item := x.(*Item)
    *pq = append(*pq, item)
}
func (pq *PriorityQueue) Pop() interface{} {
    old := *pq
    n := len(old)
    item := old[n-1]
    old[n-1] = nil
    item.index = -1
    *pq = old[0:n-1]
    return item
}
// update modifies the priority and value of an Item in the queue.
func (pq *PriorityQueue) update(i int) {
    (*pq)[i].priority += 1
    heap.Fix(pq,i)
}

func topKFrequent(nums []int, k int) []int {
    pq := PriorityQueue{}
    indices := make(map[int]int)    
    
    var n int
    for _,val := range nums {
        if i,exist := indices[val]; exist {
            pq[i].priority += 1
        }else {
            n = len(pq)
            pq = append(pq, &Item{value: val,priority: 1,index: n})
            indices[val] = n
        }
    }
    for i := 0; i < len(pq); i++ {
        fmt.Println(*pq[i])
    }
    heap.Init(&pq)
    
    ans := make([]int, k)
    for i := 0; i < k; i++ {
        ans[i] = heap.Pop(&pq).(*Item).value
    }
    return ans
}
```
