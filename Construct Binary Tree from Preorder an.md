# Construct Binary Tree from Preorder and Inorder Traversal

Given two integer arrays  `preorder`  and  `inorder`  where  `preorder`  is the preorder traversal of a binary tree and  `inorder`  is the inorder traversal of the same tree, construct and return  _the binary tree_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]

**Example 2:**

Input: preorder = [-1], inorder = [-1]
Output: [-1]

**Constraints:**

-   `1 <= preorder.length <= 3000`
-   `inorder.length == preorder.length`
-   `-3000 <= preorder[i], inorder[i] <= 3000`
-   `preorder`  and  `inorder`  consist of  **unique**  values.
-   Each value of  `inorder`  also appears in  `preorder`.
-   `preorder`  is  **guaranteed**  to be the preorder traversal of the tree.
-   `inorder`  is  **guaranteed**  to be the inorder traversal of the tree.

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
func buildTree(preorder []int, inorder []int) *TreeNode {
    n := len(preorder)
    if n == 0 { return nil }
    if n == 1 { return &TreeNode{Val: inorder[0]} }
    imap := make(map[int]int)
    for i,v := range inorder {
        imap[v] = i
    }
    
    return buildNode(preorder,imap,0,n-1,0,n-1)
}

func buildNode(preorder []int, imap map[int]int, is,ie,ps,pe int) *TreeNode {
    if is > ie || ps > pe { return nil }
    root := &TreeNode{Val: preorder[ps]}
    i := imap[preorder[ps]]
    root.Left = buildNode(preorder,imap,is,i-1,ps+1,ps+i-is)
    root.Right = buildNode(preorder,imap,i+1,ie,pe-ie+i+1,pe)
    
    return root
}
```