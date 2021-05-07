# Convert Sorted List to Binary Search Tree
Given the  `head`  of a singly linked list where elements are  **sorted in ascending order**, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of  _every_  node never differ by more than 1.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/17/linked.jpg)

    Input: head = [-10,-3,0,5,9]
    Output: [0,-3,9,-10,null,5]
    Explanation: One possible answer is [0,-3,9,-10,null,5], which represents the shown height balanced BST.

**Example 2:**

    Input: head = []
    Output: []

**Example 3:**

    Input: head = [0]
    Output: [0]

**Example 4:**

    Input: head = [1,3]
    Output: [3,1]

**Constraints:**

-   The number of nodes in  `head`  is in the range  `[0, 2 * 104]`.
-   `-10^5 <= Node.val <= 10^5`

**Solutions:**

```
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
 ```
Convert linked list to array first

```go
func sortedListToBST(head *ListNode) *TreeNode {
    if head == nil { return nil }
    nodes := []*ListNode{}
    p := head
    for p != nil {
        nodes = append(nodes, p)
        p = p.Next
    }
    
    return createTree(nodes,0,len(nodes)-1)
}

func createTree(nodes []*ListNode, start,end int) *TreeNode {
    if start > end { return nil}
    mid := (end+start)/2
    root := &TreeNode{Val: nodes[mid].Val}
    root.Left = createTree(nodes, start, mid-1)
    root.Right = createTree(nodes, mid+1, end)
    return root
}
```

2 pointers traversal

```go
func sortedListToBST(head *ListNode) *TreeNode {
    if head == nil { return nil }
 
    fast,slow := head,head
    var preslow *ListNode
    for fast != nil && fast.Next != nil {
        fast = fast.Next.Next
        preslow = slow
        slow = slow.Next
    }
    
    root := &TreeNode{Val: slow.Val}
    if preslow != nil {
        preslow.Next = nil
        root.Left = sortedListToBST(head)
        root.Right = sortedListToBST(slow.Next)
   }
    
    return root
}
```