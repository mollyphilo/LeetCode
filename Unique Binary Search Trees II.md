# Unique Binary Search Trees II
Given an integer  `n`, return  _all the structurally unique  **BST**'s (binary search trees), which has exactly_ `n` _nodes of unique values from_  `1`  _to_  `n`. Return the answer in  **any order**.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/18/uniquebstn3.jpg)

	Input: n = 3
	Output: [[1,null,2,null,3],[1,null,3,2],[2,1,3],[3,1,null,null,2],[3,2,null,1]]

**Example 2:**

	Input: n = 1
	Output: [[1]]

**Constraints:**

-   `1 <= n <= 8`

**Solution:**

Recursively form the left and right subtrees

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func generateTrees(n int) []*TreeNode {
    if n == 0 { return []*TreeNode{} }
    return helper(1,n)
}

func helper(left,right int) []*TreeNode {
    if left > right {
        return []*TreeNode{nil}
    }
    
    ans := []*TreeNode{}

    for i := left; i <= right; i++ {
        ls := helper(left,i-1)
        rs := helper(i+1,right)
        
        for li := 0; li < len(ls); li++ {
            for ri := 0; ri < len(rs); ri++ {
                ans = append(ans, &TreeNode{
                    Val: i,
                    Left: ls[li],
                    Right: rs[ri],
                })            
            }
        }
    }
    
    return ans
}
```