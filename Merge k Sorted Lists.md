# Merge k Sorted Lists
You are given an array of  `k`  linked-lists  `lists`, each linked-list is sorted in ascending order.

_Merge all the linked-lists into one sorted linked-list and return it._

**Example 1:**

	Input: lists = [[1,4,5],[1,3,4],[2,6]]
	Output: [1,1,2,3,4,4,5,6]
	Explanation: The linked-lists are:
	[
	  1->4->5,
	  1->3->4,
	  2->6
	]
	merging them into one sorted list:
	1->1->2->3->4->4->5->6

**Example 2:**

	Input: lists = []
	Output: []

**Example 3:**

	Input: lists = [[]]
	Output: []

**Constraints:**

-   `k == lists.length`
-   `0 <= k <= 10^4`
-   `0 <= lists[i].length <= 500`
-   `-10^4 <= lists[i][j] <= 10^4`
-   `lists[i]`  is sorted in  **ascending order**.
-   The sum of  `lists[i].length`  won't exceed  `10^4`.

**Solution:**
Use `merge 2 sorted lists` problem in a clever way
```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func mergeKLists(lists []*ListNode) *ListNode {
    n := len(lists)
    if n == 0 {
        return nil
    }
    if n == 1 {
        return lists[0]
    }
    
    interval := 1
    for interval < n {
        for i := 0; i < n-interval; i += 2*interval {
            lists[i] = mergeTwoLists(lists[i],lists[i+interval])
        }
        interval *= 2
    }
    
    return lists[0]
}

func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {
    head := &ListNode{Val: 0, Next: nil}
    p := head
    
    for l1 != nil && l2 != nil {
        if l1.Val <= l2.Val {
            p.Next = l1
            l1 = l1.Next
        } else {
            p.Next = l2
            l2 = l2.Next
        }
        p = p.Next
    }
    if l1 != nil {
        p.Next = l1
    } 
    if l2 != nil {
        p.Next = l2
    }
    return head.Next
}
```