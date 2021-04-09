# Increasing Order Search Tree

Given the  `root`  of a binary search tree, rearrange the tree in  **in-order**  so that the leftmost node in the tree is now the root of the tree, and every node has no left child and only one right child.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/17/ex1.jpg)

    Input: root = [5,3,6,2,4,null,8,1,null,null,null,7,9]
    Output: [1,null,2,null,3,null,4,null,5,null,6,null,7,null,8,null,9]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/17/ex2.jpg)

    Input: root = [5,1,7]
    Output: [1,null,5,null,7]

**Constraints:**

-   The number of nodes in the given tree will be in the range  `[1, 100]`.
-   `0 <= Node.val <= 1000`

---

**Solution:**

Store in-order traversal values in an array and reconstruct the tree from the array

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func increasingBST(root *TreeNode) *TreeNode {
    if root == nil { return nil }
    
    var vals []int
    inorder(root, &vals)
    
    node := &TreeNode{Val: -1}
    curr := node
    for _,v := range vals {
        curr.Right = &TreeNode{Val: v}
        curr = curr.Right
    }
    
    return node.Right
}

func inorder(root *TreeNode, vals *[]int) {
    if root == nil { return }
    
    inorder(root.Left, vals)
    *vals = append(*vals, root.Val)
    inorder(root.Right, vals)
}
```