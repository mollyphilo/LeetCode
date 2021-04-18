# Kth Largest Element in a Stream

Design a class to find the  `kth`  largest element in a stream. Note that it is the  `kth`  largest element in the sorted order, not the  `kth`  distinct element.

Implement `KthLargest` class:

-   `KthLargest(int k, int[] nums)` Initializes the object with the integer  `k`  and the stream of integers  `nums`.
-   `int add(int val)` Returns the element representing the  `kth`  largest element in the stream.

**Example 1:**

    Input
    ["KthLargest", "add", "add", "add", "add", "add"]
    [[3, [4, 5, 8, 2]], [3], [5], [10], [9], [4]]
    Output
    [null, 4, 5, 5, 8, 8]

    Explanation
    KthLargest kthLargest = new KthLargest(3, [4, 5, 8, 2]);
    kthLargest.add(3);   // return 4
    kthLargest.add(5);   // return 5
    kthLargest.add(10);  // return 5
    kthLargest.add(9);   // return 8
    kthLargest.add(4);   // return 8

**Constraints:**

-   `1 <= k <= 104`
-   `0 <= nums.length <= 104`
-   `-104  <= nums[i] <= 104`
-   `-104  <= val <= 104`
-   At most  `104`  calls will be made to  `add`.
-   It is guaranteed that there will be at least  `k`  elements in the array when you search for the  `kth`  element.

---

**Solution:**

> NOTE: 
> - Having helper's data structure and its functions compiled before the main code decrease the runtime. 
> - Only pop when necessary decreases the runtime

```go
import "container/heap"

type MinHeap []int

func (h MinHeap) Len() int { return len(h) } 
func (h MinHeap) Less(i,j int) bool { return h[i] <= h[j] }
func (h MinHeap) Swap(i,j int) { h[i],h[j] = h[j],h[i] }
func (h *MinHeap) Push(x interface{}) { *h = append(*h,x.(int)) }
func (h *MinHeap) Pop() interface{} {
    old := *h
    n := len(old)
    val := old[n-1]
    *h = old[0:n-1]
    return val
}
func (h MinHeap) Peek() int {
    return h[0]
}


type KthLargest struct {
    minheap *MinHeap
    capacity int
}


func Constructor(k int, nums []int) KthLargest {
    minheap := &MinHeap{}
    heap.Init(minheap)
    n := k 
    if n > len(nums) {
        n = len(nums)
    }
    
    for i := 0; i < n; i++ {
        heap.Push(minheap,nums[i])
    }
    
    for i := n; i < len(nums); i++ {
        heap.Push(minheap,nums[i])
        heap.Pop(minheap)
    }
    
    return KthLargest{minheap: minheap, capacity: k}
}


func (this *KthLargest) Add(val int) int {
    if this.minheap.Len() < this.capacity {
        heap.Push(this.minheap,val)
        return this.minheap.Peek()
    }
    // don't bother to push if val is less than min
    if this.minheap.Peek() < val {
        heap.Pop(this.minheap)
        heap.Push(this.minheap,val)
    }
    // because the smallest number is eventually push to the beginning of the heap array
    return this.minheap.Peek()
}

/**
 * Your KthLargest object will be instantiated and called as such:
 * obj := Constructor(k, nums);
 * param_1 := obj.Add(val);
 */
```