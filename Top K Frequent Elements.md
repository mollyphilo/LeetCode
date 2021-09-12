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

Quick select

```go
func topKFrequent(nums []int, k int) []int {
  n := len(nums)
  if n == 1 { return nums }
  freqs := make(map[int]int)
  for i := 0; i < n; i++ {
    freqs[nums[i]]++
  }
  n = len(freqs)

  arr := make([][2]int, n)
  var i int
  for k,v := range freqs {
    arr[i] = [2]int{k,v}
    i++
  }
  
  lo,hi := 0,n-1
  targetIdx := n-k
  
  for lo <= hi {
    pivot := partition(arr, lo,hi)
    if pivot < targetIdx {
      lo = pivot + 1
    }else if pivot > targetIdx {
      hi = pivot - 1
    }else {
      ans := make([]int, k)
      for i := n-1; i >= pivot; i-- {
        ans[n-1-i] = arr[i][0]
      }
      return ans
    }
  }
  
  return nums
}

func partition(nums [][2]int, lo, hi int) int {
  pivotal := nums[hi]
  swapIdx := lo
  
  for i := lo; i < hi; i++ {
    if nums[i][1] < pivotal[1] {
      nums[i],nums[swapIdx] = nums[swapIdx],nums[i]
      swapIdx++
    }
  }
  nums[hi],nums[swapIdx] = nums[swapIdx],nums[hi]
  return swapIdx
}
```

Heap

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
