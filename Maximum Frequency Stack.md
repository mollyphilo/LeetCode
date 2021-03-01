# Maximum Frequency Stack

Implement  `FreqStack`, a class which simulates the operation of a stack-like data structure.

`FreqStack` has two functions:

-   `push(int x)`, which pushes an integer  `x`  onto the stack.
-   `pop()`, which  **removes**  and returns the most frequent element in the stack.
    -   If there is a tie for most frequent element, the element closest to the top of the stack is removed and returned.

**Example 1:**

	Input:
	["FreqStack","push","push","push","push","push","push","pop","pop","pop","pop"],
	[[],[5],[7],[5],[7],[4],[5],[],[],[],[]]
	Output: [null,null,null,null,null,null,null,5,7,5,4]
	Explanation:
	After making six .push operations, the stack is [5,7,5,7,4,5] from bottom to top.  Then:

	pop() -> returns 5, as 5 is the most frequent.
	The stack becomes [5,7,5,7,4].

	pop() -> returns 7, as 5 and 7 is the most frequent, but 7 is closest to the top.
	The stack becomes [5,7,5,4].

	pop() -> returns 5.
	The stack becomes [5,7,4].

	pop() -> returns 4.
	The stack becomes [5,7].

**Note:**

-   Calls to  `FreqStack.push(int x)` will be such that  `0 <= x <= 10^9`.
-   It is guaranteed that  `FreqStack.pop()`  won't be called if the stack has zero elements.
-   The total number of  `FreqStack.push`  calls will not exceed  `10000`  in a single test case.
-   The total number of  `FreqStack.pop` calls will not exceed  `10000`  in a single test case.
-   The total number of  `FreqStack.push`  and  `FreqStack.pop`  calls will not exceed  `150000`  across all test cases.

**Solution:**

Stack of priority queue

```go
import "container/heap"

type FreqNum struct {
    val,count,order int
}

type PriorityQueue []*FreqNum

func (pq PriorityQueue) Len() int { return len(pq) }
func (pq PriorityQueue) Less(i,j int) bool {
    if pq[i].count == pq[j].count {
        return pq[i].order > pq[j].order
    }
    return pq[i].count > pq[j].count 
}
func (pq PriorityQueue) Swap(i,j int) { pq[i],pq[j] = pq[j],pq[i] }
func (pq *PriorityQueue) Push(x interface{}) {
    item := x.(*FreqNum)
    *pq = append(*pq, item)
}
func (pq *PriorityQueue) Pop() interface{} {
    old := *pq
    n := len(old)
    item := old[n-1]
    old[n-1] = nil
    *pq = old[0:n-1]
    return item
}

type FreqStack struct {
    items *PriorityQueue
    counters map[int]int
    indexes int
}


func Constructor() FreqStack {
    fs := FreqStack{items: &PriorityQueue{}, counters: make(map[int]int)}
    heap.Init(fs.items)
    return fs
}


func (this *FreqStack) Push(x int)  {
    this.indexes += 1
    this.counters[x] += 1
    item := &FreqNum{val: x, count: this.counters[x], order: this.indexes}
    heap.Push(this.items, item)
}


func (this *FreqStack) Pop() int {
    item := heap.Pop(this.items).(*FreqNum)
    this.counters[item.val] -= 1
    return item.val
}


/**
 * Your FreqStack object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Push(x);
 * param_2 := obj.Pop();
 */
```

LeetCode has a slightly nicer solution with stack of stack, but it is even slower than the previous solution

```go
type FreqStack struct {
    freq map[int]int
    groupByFreq map[int][]int
    maxFreq int
}


func Constructor() FreqStack {
    fs := FreqStack{groupByFreq: make(map[int][]int), maxFreq: 0, freq: make(map[int]int)}
    return fs
}


func (this *FreqStack) Push(x int)  {
    this.freq[x] += 1
    v := this.freq[x]
    if v > this.maxFreq {
        this.maxFreq = v
    }
    this.groupByFreq[v] = append(this.groupByFreq[v],x)
}


func (this *FreqStack) Pop() int {
    maxGroup := this.groupByFreq[this.maxFreq]
    n,x := len(maxGroup)-1,0
    x,this.groupByFreq[this.maxFreq] = maxGroup[n],maxGroup[:n]
    if n == 0 {
        this.maxFreq -= 1
    }
    
    this.freq[x] -= 1
    return x
}


/**
 * Your FreqStack object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Push(x);
 * param_2 := obj.Pop();
 */
```