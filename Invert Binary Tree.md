# 226.  Invert Binary Tree

Given the  `root`  of a binary tree, invert the tree, and return  _its root_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg)

	Input: root = [4,2,7,1,3,6,9]
	Output: [4,7,2,9,6,3,1]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/14/invert2-tree.jpg)

	Input: root = [2,1,3]
	Output: [2,3,1]

**Example 3:**

	Input: root = []
	Output: []

**Constraints:**

-   The number of nodes in the tree is in the range  `[0, 100]`.
-   `-100 <= Node.val <= 100`

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
func invertTree(root *TreeNode) *TreeNode {
    if root == nil { return nil }
    stack := []*TreeNode{root}
    
    var node *TreeNode
    
    for len(stack) > 0 {
        n := len(stack)
        var temp []*TreeNode
        for i := 0; i < n; i++ {
            node = stack[i]
            node.Left,node.Right = node.Right,node.Left
            if node.Left != nil {
                temp = append(temp, node.Left)    
            }
            if node.Right != nil {
                temp = append(temp, node.Right)    
            }
        }
        
        stack = temp
    }
    
    return root
}
```