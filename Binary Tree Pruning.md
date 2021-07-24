# Binary Tree Pruning

Given the  `root`  of a binary tree, return  _the same tree where every subtree (of the given tree) not containing a_ `1` _has been removed_.

A subtree of a node  `node`  is  `node`  plus every node that is a descendant of  `node`.

**Example 1:**

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/06/1028_2.png)

**Input:** root = [1,null,0,0,1]
**Output:** [1,null,0,null,1]
**Explanation:** 
Only the red nodes satisfy the property "every subtree not containing a 1".
The diagram on the right represents the answer.

**Example 2:**

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/06/1028_1.png)

**Input:** root = [1,0,1,0,0,0,1]
**Output:** [1,null,1,null,1]

**Example 3:**

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/05/1028.png)

**Input:** root = [1,1,0,1,1,0,1,0]
**Output:** [1,1,0,1,1,null,1]

**Constraints:**

-   The number of nodes in the tree is in the range  `[1, 200]`.
-   `Node.val`  is either  `0`  or  `1`.


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
func pruneTree(root *TreeNode) *TreeNode {
    if root == nil { return root }
    root,_ = has_one(root)
    return root
}

func has_one(root *TreeNode) (*TreeNode, bool) {
    if root == nil { return nil, false }
    
    left,onLeft := has_one(root.Left)
    if !onLeft {
        root.Left = nil
    }else {
        root.Left = left
    }
    
    right,onRight := has_one(root.Right)
    if !onRight {
        root.Right = nil
    }else {
        root.Right = right
    }
    
    if onLeft || onRight || root.Val == 1 {
        return root, true
    }
    
    return nil, false
}
```
