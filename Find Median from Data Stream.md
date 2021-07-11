# Find Median from Data Stream

The  **median**  is the middle value in an ordered integer list. If the size of the list is even, there is no middle value and the median is the mean of the two middle values.

-   For example, for  `arr = [2,3,4]`, the median is  `3`.
-   For example, for  `arr = [2,3]`, the median is  `(2 + 3) / 2 = 2.5`.

Implement the MedianFinder class:

-   `MedianFinder()`  initializes the  `MedianFinder`  object.
-   `void addNum(int num)`  adds the integer  `num`  from the data stream to the data structure.
-   `double findMedian()`  returns the median of all elements so far. Answers within  `10-5`  of the actual answer will be accepted.

**Example 1:**

	Input
	["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]
	[[], [1], [2], [], [3], []]
	Output
	[null, null, null, 1.5, null, 2.0]

	Explanation
	MedianFinder medianFinder = new MedianFinder();
	medianFinder.addNum(1);    // arr = [1]
	medianFinder.addNum(2);    // arr = [1, 2]
	medianFinder.findMedian(); // return 1.5 (i.e., (1 + 2) / 2)
	medianFinder.addNum(3);    // arr[1, 2, 3]
	medianFinder.findMedian(); // return 2.0

**Constraints:**

-   `-105  <= num <= 105`
-   There will be at least one element in the data structure before calling  `findMedian`.
-   At most  `5 * 104`  calls will be made to  `addNum`  and  `findMedian`.

**Follow up:**

-   If all integer numbers from the stream are in the range  `[0, 100]`, how would you optimize your solution?
-   If  `99%`  of all integer numbers from the stream are in the range  `[0, 100]`, how would you optimize your solution?

**Solution:**

### Binary Search
```go
type MedianFinder struct {
    nums []int
}


/** initialize your data structure here. */
func Constructor() MedianFinder {
    return MedianFinder{nums: nil}
}


func (this *MedianFinder) AddNum(num int)  {
    if this.nums == nil {
        this.nums = []int{num}
        return
    }
    index := binarySearch(this.nums, num)
    temp := make([]int, len(this.nums) + 1)
    copy(temp[0:index],this.nums[0:index])
    temp[index] = num
    copy(temp[index+1:], this.nums[index:])
    this.nums = temp
}


func (this *MedianFinder) FindMedian() float64 {
    mid := len(this.nums)/2
    if len(this.nums) % 2 == 0 {
        return float64(this.nums[mid]+this.nums[mid-1])/2
    }
    return float64(this.nums[mid])
}

func binarySearch(nums []int, num int) int {
    n := len(nums)
    low,high,mid := 0,n-1,-1
    for low <= high {
        mid = (low+high)/2
        if nums[mid] == num { 
            return mid 
        } else if nums[mid] > num { 
            high = mid-1
        } else {
            low = mid+1
        }
    }
    if nums[mid] > num {
        return mid
    }
    return mid+1
}



/**
 * Your MedianFinder object will be instantiated and called as such:
 * obj := Constructor();
 * obj.AddNum(num);
 * param_2 := obj.FindMedian();
 */
```

### MinHeap for upper half and MaxHeap for lower half

```go
import "container/heap"

type MaxHeap []int
func (h MaxHeap) Len() int { return len(h) }
func (h MaxHeap) Less(i,j int) bool { return h[i] > h[j] }
func (h MaxHeap) Swap(i,j int) { h[i],h[j] = h[j],h[i] }
func (h MaxHeap) Peek() interface{} { return h[0] }
func (h *MaxHeap) Push(x interface{}) { *h = append(*h, x.(int)) }
func (h *MaxHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0:n-1]
    return x
}

type MinHeap []int
func (h MinHeap) Len() int { return len(h) }
func (h MinHeap) Less(i,j int) bool { return h[i] < h[j] }
func (h MinHeap) Swap(i,j int) { h[i],h[j] = h[j],h[i] }
func (h MinHeap) Peek() interface{} { return h[0] }
func (h *MinHeap) Push(x interface{}) { *h = append(*h, x.(int)) }
func (h *MinHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0:n-1]
    return x
}

type MedianFinder struct {
    lowerHalf MaxHeap
    upperHalf MinHeap
}


/** initialize your data structure here. */
func Constructor() MedianFinder {
    mf := MedianFinder{
        lowerHalf: MaxHeap{},
        upperHalf: MinHeap{},
    }
    heap.Init(&mf.lowerHalf)
    heap.Init(&mf.upperHalf)
    return mf
}


func (this *MedianFinder) AddNum(num int)  {
    heap.Push(&this.lowerHalf, num)
    // rebalance
    heap.Push(&this.upperHalf, heap.Pop(&this.lowerHalf))
    if this.upperHalf.Len() > this.lowerHalf.Len() {
        heap.Push(&this.lowerHalf, heap.Pop(&this.upperHalf))
    }
}


func (this *MedianFinder) FindMedian() float64 {
    if this.lowerHalf.Len() > this.upperHalf.Len() {
        return float64(this.lowerHalf.Peek().(int))
    }
    return float64(this.lowerHalf.Peek().(int) + this.upperHalf.Peek().(int))/2
}


/**
 * Your MedianFinder object will be instantiated and called as such:
 * obj := Constructor();
 * obj.AddNum(num);
 * param_2 := obj.FindMedian();
 */
```
