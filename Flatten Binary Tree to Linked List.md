# Flatten Binary Tree to Linked List

Given the  `root`  of a binary tree, flatten the tree into a "linked list":

-   The "linked list" should use the same  `TreeNode`  class where the  `right`  child pointer points to the next node in the list and the  `left`  child pointer is always  `null`.
-   The "linked list" should be in the same order as a  [**pre-order** **traversal**](https://en.wikipedia.org/wiki/Tree_traversal#Pre-order,_NLR)  of the binary tree.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/14/flaten.jpg)

	Input: root = [1,2,5,3,4,null,6]
	Output: [1,null,2,null,3,null,4,null,5,null,6]

**Example 2:**

	Input: root = []
	Output: []

**Example 3:**

	Input: root = [0]
	Output: [0]

**Constraints:**

-   The number of nodes in the tree is in the range  `[0, 2000]`.
-   `-100 <= Node.val <= 100`

**Follow up:** Can you flatten the tree in-place (with `O(1)` extra space)?

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
func flatten(root *TreeNode)  {
    if root == nil { return }
    flattenNode(root)
}

func flattenNode(root *TreeNode) *TreeNode {
    if root == nil { return root }

    left,right := flattenNode(root.Left),flattenNode(root.Right)
    if left != nil {
        left.Left = nil
        left.Right = root.Right
        root.Right = root.Left
        root.Left = nil
    }
    if right != nil { return right }
    if left != nil { return left }   
    return root
}
```

Slightly different

```go
func flatten(root *TreeNode)  {
    if root == nil { return }
    
    traverse(root)
}

func traverse(root *TreeNode) *TreeNode {
    if root == nil || (root.Left == nil && root.Right == nil) { return root }
    
    rightStart := root.Right
    if root.Left != nil {
        leftEnd := traverse(root.Left)
        root.Right = root.Left
        root.Left = nil
        if rightStart == nil {
            return leftEnd
        }
        leftEnd.Right = rightStart
    }
    return traverse(rightStart)
}
```