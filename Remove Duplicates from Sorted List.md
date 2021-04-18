# Remove Duplicates from Sorted List

Given the  `head`  of a sorted linked list,  _delete all duplicates such that each element appears only once_. Return  _the linked list  **sorted**  as well_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/04/list1.jpg)

    Input: head = [1,1,2]
    Output: [1,2]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/04/list2.jpg)

    Input: head = [1,1,2,3,3]
    Output: [1,2,3]

**Constraints:**

-   The number of nodes in the list is in the range  `[0, 300]`.
-   `-100 <= Node.val <= 100`
-   The list is guaranteed to be  **sorted**  in ascending order.

---

**Solutions:**

With hashmap

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func deleteDuplicates(head *ListNode) *ListNode {
    if head == nil {
        return nil
    }
    tb := make(map[int]struct{})
    
    d,prev := head,head
    tb[d.Val] = struct{}{}
    for d != nil {
        if _,exist := tb[d.Val]; exist {
            prev.Next = d.Next
        }else {
            tb[d.Val] = struct{}{}
            prev = d
        }
        d = d.Next
    }
    
    return head
}
```

With new node

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func deleteDuplicates(head *ListNode) *ListNode {
    if head == nil {
        return nil
    }
    ans := head
    dummy := ans
    node := head
    
    var val int
    for node != nil {
        val = node.Val
        node = node.Next
        for node != nil && node.Val == val {
            node = node.Next
        }
        dummy.Next = node
        dummy = dummy.Next
    }
    return ans
}
```