# Path Sum II

Given the  `root`  of a binary tree and an integer  `targetSum`, return all  **root-to-leaf**  paths where each path's sum equals  `targetSum`.

A  **leaf**  is a node with no children.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/18/pathsumii1.jpg)

	Input: root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
	Output: [[5,4,11,2],[5,8,4,5]]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/18/pathsum2.jpg)

	Input: root = [1,2,3], targetSum = 5
	Output: []

**Example 3:**

	Input: root = [1,2], targetSum = 0
	Output: []

**Constraints:**

-   The number of nodes in the tree is in the range  `[0, 5000]`.
-   `-1000 <= Node.val <= 1000`
-   `-1000 <= targetSum <= 1000`

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
func pathSum(root *TreeNode, targetSum int) [][]int {
    if root == nil { return [][]int{} }
    var res [][]int
    backtrack(root,targetSum, &res, []int{root.Val}, root.Val)
    return res
}

func backtrack(root *TreeNode, target int, res *[][]int, cur []int, sum int) {
    if root == nil { return }
    // leaf node
    if root.Left == nil && root.Right == nil {
        if sum == target && len(cur) > 0 {
            temp := make([]int, len(cur))
            copy(temp,cur)
            *res = append(*res, temp)
        }
        return
    }
    if root.Left != nil {
        backtrack(root.Left, target, res, append(cur, root.Left.Val), sum + root.Left.Val)
    }
    if root.Right != nil {
        backtrack(root.Right, target, res, append(cur, root.Right.Val), sum + root.Right.Val)
    }
}
```