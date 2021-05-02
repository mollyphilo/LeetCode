# Deepest Leaves Sum

Given the `root` of a binary tree, return _the sum of values of its deepest leaves_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/07/31/1483_ex1.png)

    Input: root = [1,2,3,4,5,null,6,7,null,null,null,null,8]
    Output: 15

**Example 2:**

    Input: root = [6,7,8,2,7,1,3,9,null,1,4,null,null,null,5]
    Output: 19

**Constraints:**

-   The number of nodes in the tree is in the range  `[1, 104]`.
-   `1 <= Node.val <= 100`

---

**Solutions:**

*Approach 1:*

Get all leave nodes and sum those with same deepest height

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */

func deepestLeavesSum(root *TreeNode) int {
    if root == nil { return 0 }

    var vals [][2]int
    walk(root, &vals, 0)
    
    var sum,maxHeight int
    for i := 0; i < len(vals); i++ {
        if vals[i][0] > maxHeight {
            maxHeight = vals[i][0]
            sum = vals[i][1]
        }else if vals[i][0] == maxHeight {
            sum += vals[i][1]
        }
    }
    
    return sum
}

func walk(node *TreeNode, vals *[][2]int, height int) {
    if node == nil { return }
    if node.Left == nil && node.Right == nil {
        *vals = append(*vals, [2]int{height, node.Val})
        return
    }
    walk(node.Left, vals, height + 1)
    walk(node.Right, vals, height + 1)
}
```

*Approach 2:*

Get the deepest height and traverse again to get all nodes at that height

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */

func deepestLeavesSum(root *TreeNode) int {
    if root == nil { return 0 }

    height := getDeepestHeight(root,0)
    
    return sumAtDeepestHeight(root,height)
}

func sumAtDeepestHeight(root *TreeNode, dHeight int) int {
    if root == nil { return 0 }
    
    if dHeight == 0 {
        return root.Val
    }
    
    var sum int
    if root.Left != nil {
        sum += sumAtDeepestHeight(root.Left, dHeight - 1)    
    }
    
    if root.Right != nil {
        sum += sumAtDeepestHeight(root.Right, dHeight - 1)    
    }
    
    return sum
}

func getDeepestHeight(node *TreeNode, height int) int {
    if node == nil { return 0 }
    
    if node.Left == nil && node.Right == nil {
        return height
    }
    
    var l,r int
    if node.Left != nil {
        l = getDeepestHeight(node.Left, height + 1)
    }
    if node.Right != nil {
        r = getDeepestHeight(node.Right, height + 1)
    }
    
    if l < r {
        return r
    }
    
    return l
}
```