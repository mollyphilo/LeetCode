# 437.  Path Sum III

Given the  `root`  of a binary tree and an integer  `targetSum`, return  _the number of paths where the sum of the values along the path equals_ `targetSum`.

The path does not need to start or end at the root or a leaf, but it must go downwards (i.e., traveling only from parent nodes to child nodes).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/09/pathsum3-1-tree.jpg)

	Input: root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8
	Output: 3
	Explanation: The paths that sum to 8 are shown.

**Example 2:**

	Input: root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
	Output: 3

**Constraints:**

-   The number of nodes in the tree is in the range  `[0, 1000]`.
-   `-109  <= Node.val <= 109`
-   `-1000 <= targetSum <= 1000`

**Solution:**

Should check https://leetcode.com/problems/path-sum-iii/discuss/534823/Summary-of-4-solutions-(-keypoints-in-both-English-and-Chinese-)

Time & space complexity: O(N)

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func pathSum(root *TreeNode, targetSum int) int {
    if root == nil { return 0 }
    if root.Left == nil && root.Right == nil && root.Val == targetSum { return 1 }

    var ans int
    subsum := map[int]int{0: 1}
    backtrack(root, targetSum, 0, subsum, &ans)
    
    return ans
}

func backtrack(root *TreeNode, targetSum int, sum int, subsum map[int]int, ans *int) {
    sum += root.Val
    if v, ok := subsum[sum - targetSum]; ok {
        *ans += v
    }

    subsum[sum]++
    
    if root.Left != nil {
        backtrack(root.Left, targetSum, sum, subsum, ans)
    }
    if root.Right != nil {
        backtrack(root.Right, targetSum, sum, subsum, ans)
    }
    subsum[sum]--
}
```