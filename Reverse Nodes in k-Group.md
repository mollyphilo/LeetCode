# Reverse Nodes in k-Group

Given a linked list, reverse the nodes of a linked list  _k_  at a time and return its modified list.

_k_  is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of  _k_  then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex1.jpg)

	Input: head = [1,2,3,4,5], k = 2
	Output: [2,1,4,3,5]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex2.jpg)

	Input: head = [1,2,3,4,5], k = 3
	Output: [3,2,1,4,5]

**Example 3:**

	Input: head = [1,2,3,4,5], k = 1
	Output: [1,2,3,4,5]

**Example 4:**

	Input: head = [1], k = 1
	Output: [1]

**Constraints:**

-   The number of nodes in the list is in the range  `sz`.
-   `1 <= sz <= 5000`
-   `0 <= Node.val <= 1000`
-   `1 <= k <= sz`

**Follow-up:** Can you solve the problem in O(1) extra memory space?

**Solution:**

O(N) space complexity and O(N) time complexity: store values of nodes and creates new LinkedList

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reverseKGroup(head *ListNode, k int) *ListNode {
    var values []int
    node := head
    for node != nil {
        values = append(values, node.Val)
        node = node.Next
    }
    dummy := &ListNode{Val: 0}
    ans := dummy
    node = head
    var i int
    for i + k <= len(values) {
        for j := k; j > 0; j-- {
            dummy.Next = &ListNode{Val: values[i+j-1]}
            dummy = dummy.Next
        }
        i += k
    }
    for i < len(values) {
        dummy.Next = &ListNode{Val: values[i]}
        dummy = dummy.Next
        i++
    }
    return ans.Next
}
```

Another solution with space complexity O(1)

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reverseKGroup(head *ListNode, k int) *ListNode {
    // let [1 2 3 4 5], k=2 be an example
    var count int
    node := head
    count = 1
    for count < k && node != nil {
        node = node.Next
        count++
    }
    // node should be 3 here
    if count != k || node == nil {
        return head
    }
    // prev should be the head node of 2->1
    // curr should be the head node of 3->4->5
    prev,curr := reverseKNodes(head,k)
    // head is 1, so it's now the end of prev
    // so head.Next should be new reverseKGroup from 3
    head.Next = reverseKGroup(curr,k)
    
    return prev
}

func reverseKNodes(head *ListNode, k int) (*ListNode,*ListNode) {
    curr := head
    var prev,next *ListNode
    for i := 0; i < k; i++ {
        next = curr.Next
        curr.Next = prev
        prev = curr
        curr = next
    }
    
    return prev,curr
}
```