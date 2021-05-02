# Reverse Linked List

Given the  `head`  of a singly linked list, reverse the list, and return  _the reversed list_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

    Input: head = [1,2,3,4,5]
    Output: [5,4,3,2,1]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)

    Input: head = [1,2]
    Output: [2,1]

**Example 3:**

    Input: head = []
    Output: []

**Constraints:**

-   The number of nodes in the list is the range  `[0, 5000]`.
-   `-5000 <= Node.val <= 5000`

**Follow up:**  A linked list can be reversed either iteratively or recursively. Could you implement both?

---

**Solutions:**

Approach 1: using additional data structure stack

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reverseList(head *ListNode) *ListNode {
    if head == nil { return nil }
    
    var stack []*ListNode
    
    node := head
    for node != nil {
        stack = append(stack, node)
        node = node.Next
    }
    
    node = &ListNode{Val: -1, Next: nil}
    dummy := node
    for i := len(stack)-1; i >= 0; i-- {
        dummy.Next = stack[i]
        dummy = dummy.Next
    }
    
    dummy.Next = nil
    
    return node.Next
}
```

Approach 2: additional variable `prev` to reverse the linked list while traversing

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reverseList(head *ListNode) *ListNode {
    if head == nil { return nil }
    
    cur := head
    var prev,temp *ListNode
    
    for cur != nil {
        temp = cur
        cur = cur.Next
        temp.Next = prev
        prev = temp
    }
    
    return prev
}
```