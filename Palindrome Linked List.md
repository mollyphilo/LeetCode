# Palindrome Linked List

Given the  `head`  of a singly linked list, return  `true`  if it is a palindrome.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/03/pal1linked-list.jpg)

	Input: head = [1,2,2,1]
	Output: true

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/03/pal2linked-list.jpg)

	Input: head = [1,2]
	Output: false

**Constraints:**

-   The number of nodes in the list is in the range  `[1, 105]`.
-   `0 <= Node.val <= 9`

**Follow up:** Could you do it in `O(n)` time and `O(1)` space?

**Solution:**

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */

func isPalindrome(head *ListNode) bool {
    // find the middle element
    slow,fast := head,head
    for fast != nil && fast.Next != nil {
        slow = slow.Next
        fast = fast.Next.Next
    }
    // length is odd, we compare left half and right half without middle element
    if fast != nil {
        slow = slow.Next 
    }
    
    // reverse slow linkedlist
    var curr,prev,temp *ListNode = slow,nil,nil
    for curr != nil {
        temp = curr.Next
        curr.Next = prev
        prev = curr
        curr = temp
    }
    
    // compare 2 parts
    for prev != nil {
        if prev.Val != head.Val {
            return false
        }
        prev = prev.Next
        head = head.Next
    }
    
    return true
}
```