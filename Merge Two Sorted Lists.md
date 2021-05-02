# Merge Two Sorted Lists
Merge two sorted linked lists and return it as a  **sorted**  list. The list should be made by splicing together the nodes of the first two lists.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg)

    Input: l1 = [1,2,4], l2 = [1,3,4]
    Output: [1,1,2,3,4,4]

**Example 2:**

    Input: l1 = [], l2 = []
    Output: []

**Example 3:**

    Input: l1 = [], l2 = [0]
    Output: [0]

**Constraints:**

-   The number of nodes in both lists is in the range  `[0, 50]`.
-   `-100 <= Node.val <= 100`
-   Both  `l1`  and  `l2`  are sorted in  **non-decreasing**  order.

**Solution:**

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
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