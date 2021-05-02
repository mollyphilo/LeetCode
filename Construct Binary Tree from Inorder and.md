# Construct Binary Tree from Inorder and Postorder Traversal

Given two integer arrays  `inorder`  and  `postorder`  where  `inorder`  is the inorder traversal of a binary tree and  `postorder`  is the postorder traversal of the same tree, construct and return  _the binary tree_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

    Input: inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
    Output: [3,9,20,null,null,15,7]

**Example 2:**

    Input: inorder = [-1], postorder = [-1]
    Output: [-1]

**Constraints:**

-   `1 <= inorder.length <= 3000`
-   `postorder.length == inorder.length`
-   `-3000 <= inorder[i], postorder[i] <= 3000`
-   `inorder`  and  `postorder`  consist of  **unique**  values.
-   Each value of  `postorder`  also appears in  `inorder`.
-   `inorder`  is  **guaranteed**  to be the inorder traversal of the tree.
-   `postorder`  is  **guaranteed**  to be the postorder traversal of the tree.

**Solution:**

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func buildTree(inorder []int, postorder []int) *TreeNode {
    n := len(postorder)
    if n == 0 { return nil }
    if n == 1 { return &TreeNode{Val: inorder[0] } }
    
    imap := make(map[int]int)
    for i,v := range inorder {
        imap[v] = i
    }
    return buildNode(inorder,postorder,imap,0,n-1,0,n-1)
}

func buildNode(inorder,postorder []int, imap map[int]int, is,ie,ps,pe int) *TreeNode {
    if is > ie || ps > pe { return nil }

    root := &TreeNode{Val: postorder[pe] }
    ir := imap[postorder[pe]]
    root.Left = buildNode(inorder,postorder,imap,is,ir-1,ps,ps+ir-1-is)
    root.Right = buildNode(inorder,postorder,imap,ir+1,ie,pe-ie+ir,pe-1)
    
    return root
}
```