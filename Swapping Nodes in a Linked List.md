# Swapping Nodes in a Linked List
You are given the  `head`  of a linked list, and an integer  `k`.

Return  _the head of the linked list after  **swapping**  the values of the_ `kth`  _node from the beginning and the_ `kth`  _node from the end (the list is  **1-indexed**)._

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/21/linked1.jpg)

    Input: head = [1,2,3,4,5], k = 2
    Output: [1,4,3,2,5]

**Example 2:**

    Input: head = [7,9,6,6,7,8,3,0,9,5], k = 5
    Output: [7,9,6,6,8,7,3,0,9,5]

**Example 3:**

    Input: head = [1], k = 1
    Output: [1]

**Example 4:**

    Input: head = [1,2], k = 1
    Output: [2,1]

**Example 5:**

    Input: head = [1,2,3], k = 2
    Output: [1,2,3]

**Constraints:**

-   The number of nodes in the list is  `n`.
-   `1 <= k <= n <= 105`
-   `0 <= Node.val <= 100`

**Solution:**

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func swapNodes(head *ListNode, k int) *ListNode {
    var front *ListNode
    node := &ListNode{Val: 0, Next:head}
    var n,count int = 0,0
    // find the kth node from the head
    for node.Next != nil {
        if count == k {
            front = node
        }
        count++
        n++
        node = node.Next
    }
    if node.Next == nil && node != nil && count == k {
        front = node
    }
    n -= k - 1
    count = 1
    node = head
    // the kth node from the tail is at (n-k+1)th node
    for node != nil && count < n {
        node = node.Next
        count++
    }
    // back = node
    temp := front.Val
    front.Val = node.Val
    node.Val = temp
    return head
}
```