# My Calendar I

You are implementing a program to use as your calendar. We can add a new event if adding the event will not cause a  **double booking**.

A  **double booking**  happens when two events have some non-empty intersection (i.e., some moment is common to both events.).

The event can be represented as a pair of integers  `start`  and  `end`  that represents a booking on the half-open interval  `[start, end)`, the range of real numbers  `x`  such that  `start <= x < end`.

Implement the  `MyCalendar`  class:

-   `MyCalendar()`  Initializes the calendar object.
-   `boolean book(int start, int end)`  Returns  `true`  if the event can be added to the calendar successfully without causing a  **double booking**. Otherwise, return  `false`  and do not add the event to the calendar.

**Example 1:**

	Input:
	["MyCalendar", "book", "book", "book"]
	[[], [10, 20], [15, 25], [20, 30]]
	Output
	[null, true, false, true]

	Explanation:
	MyCalendar myCalendar = new MyCalendar();
	myCalendar.book(10, 20); // return True
	myCalendar.book(15, 25); // return False, It can not be booked because time 15 is already booked by another event.
	myCalendar.book(20, 30); // return True, The event can be booked, as the first event takes every time less than 20, but not including 20.

**Constraints:**

-   `0 <= start < end <= 109`
-   At most  `1000`  calls will be made to  `book`.

**Solution:**

```go
type Node struct {
    start,end int
    left,right *Node
}

type MyCalendar struct {
    root *Node
}


func Constructor() MyCalendar {
    return MyCalendar{root: nil}
}


func (this *MyCalendar) Book(start int, end int) bool {
    node,added := buildTree(this.root,start,end)
    if added {
        this.root = node
    }
    return added
}

func buildTree(root *Node, start,end int) (*Node,bool) {
    if root == nil {
        return &Node{start: start, end: end},true
    }
    
    if root.start >= end {
        // left     
        node,added := buildTree(root.left,start,end)
        if added {
            root.left = node
        }
        return root,added
    }else if root.end <= start {
        node,added := buildTree(root.right,start,end)
        if added {
            root.right = node
        }
        return root,added
    }
    return root,false
}
/**
 * Your MyCalendar object will be instantiated and called as such:
 * obj := Constructor();
 * param_1 := obj.Book(start,end);
 */
```