# Reverse Linked List II

Given the  `head`  of a singly linked list and two integers  `left`  and  `right`  where  `left <= right`, reverse the nodes of the list from position  `left`  to position  `right`, and return  _the reversed list_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/rev2ex2.jpg)

	Input: head = [1,2,3,4,5], left = 2, right = 4
	Output: [1,4,3,2,5]

**Example 2:**

	Input: head = [5], left = 1, right = 1
	Output: [5]

**Constraints:**

-   The number of nodes in the list is  `n`.
-   `1 <= n <= 500`
-   `-500 <= Node.val <= 500`
-   `1 <= left <= right <= n`

**Follow up:** Could you do it in one pass?

**Solution:**

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reverseBetween(head *ListNode, left int, right int) *ListNode {
    dummy := &ListNode{Next: head} 
    node := dummy
    pos := 0
    for node.Next != nil && pos + 1 != left {
        node = node.Next
        pos++
    }
    if node.Next != nil && pos + 1 == left {
        var prev,next *ListNode
        curr := node.Next
        for curr != nil {
            next = curr.Next
            curr.Next = prev
            prev = curr
            curr = next
            pos++
            if pos == right { break }
        }

        node.Next.Next = curr
        node.Next = prev
    }
    return dummy.Next
}
```