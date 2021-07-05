# My Calendar II
You are implementing a program to use as your calendar. We can add a new event if adding the event will not cause a  **triple booking**.

A  **triple booking**  happens when three events have some non-empty intersection (i.e., some moment is common to all the three events.).

The event can be represented as a pair of integers  `start`  and  `end`  that represents a booking on the half-open interval  `[start, end)`, the range of real numbers  `x`  such that  `start <= x < end`.

Implement the  `MyCalendarTwo`  class:

-   `MyCalendarTwo()`  Initializes the calendar object.
-   `boolean book(int start, int end)`  Returns  `true`  if the event can be added to the calendar successfully without causing a  **triple booking**. Otherwise, return  `false`  and do not add the event to the calendar.

**Example 1:**

	Input
	["MyCalendarTwo", "book", "book", "book", "book", "book", "book"]
	[[], [10, 20], [50, 60], [10, 40], [5, 15], [5, 10], [25, 55]]
	Output
	[null, true, true, true, false, true, true]

**Explanation**

	MyCalendarTwo myCalendarTwo = new MyCalendarTwo();
	myCalendarTwo.book(10, 20); // return True, The event can be booked. 
	myCalendarTwo.book(50, 60); // return True, The event can be booked. 
	myCalendarTwo.book(10, 40); // return True, The event can be double booked. 
	myCalendarTwo.book(5, 15);  // return False, The event ca not be booked, because it would result in a triple booking.
	myCalendarTwo.book(5, 10); // return True, The event can be booked, as it does not use time 10 which is already double booked.
	myCalendarTwo.book(25, 55); // return True, The event can be booked, as the time in [25, 40) will be double booked with the third event, the time [40, 50) will be single booked, and the time [50, 55) will be double booked with the second event.

**Constraints:**

-   `0 <= start < end <= 109`
-   At most  `1000`  calls will be made to  `book`.

**Solution:**

Segment Tree:
* Time Complexity: O(logN) on average and O(N) on worst case of each query. same for update
* Space Complexity: O(K) with K is number of less than 3 overlapped blocks

```go
type TreeNode struct {
    start,end,events int // start is inclusive, end is exclusive
    left,right *TreeNode
}

type MyCalendarTwo struct {
    root *TreeNode
}


func Constructor() MyCalendarTwo {
    return MyCalendarTwo{}
}


func (this *MyCalendarTwo) Book(start int, end int) bool {
    if query(this.root, start,end) >= 2 { return false }
    this.root = update(this.root, start,end)
    return true
}

func query(root *TreeNode, start, end int) int {
    if start >= end || root == nil { return 0 }
    
    if root.start >= end { return query(root.left, start,end) }
    if root.end <= start { return query(root.right, start,end) }
    
    return max(root.events, query(root.left, start,end), query(root.right, start,end))
}

func update(root *TreeNode, start, end int) *TreeNode {
    if start >= end   { return root }
    if root == nil { return &TreeNode{start: start,end: end,events: 1} }
    if root.end <= start { 
        root.right = update(root.right, start,end)
    }else if root.start >= end {
        root.left = update(root.left, start,end)
    }else {
        minstart,maxstart,minend,maxend := root.start,root.start,root.end,root.end
        if start < minstart { minstart = start }
        if start > maxstart { maxstart = start }
        if end < minend { minend = end }
        if end > maxend { maxend = end }
        
        root.start,root.end = maxstart,minend
        root.events += 1
        
        root.left = update(root.left, minstart, maxstart)
        root.right = update(root.right, minend, maxend)
    }
    
    return root
}

func max(nums... int) int {
    var max int
    for i := 0; i < len(nums); i++ {
        if nums[i] > max { max = nums[i] }
    }
    
    return max
}


/**
 * Your MyCalendarTwo object will be instantiated and called as such:
 * obj := Constructor();
 * param_1 := obj.Book(start,end);
 */
```