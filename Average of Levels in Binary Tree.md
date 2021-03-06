# Average of Levels in Binary Tree
Given a non-empty binary tree, return the average value of the nodes on each level in the form of an array.

**Example 1:**  

	Input:
	    3
	   / \
	  9  20
	    /  \
	   15   7
	Output: [3, 14.5, 11]
	Explanation:
	The average value of nodes on level 0 is 3,  on level 1 is 14.5, and on level 2 is 11. Hence return [3, 14.5, 11].

**Note:**  

1.  The range of node's value is in the range of 32-bit signed integer.

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
// BFS traverses by level
func averageOfLevels(root *TreeNode) []float64 {
    var ans []float64
    if root == nil { return ans }
    q  := []*TreeNode{root}
    
    var node *TreeNode
    var sum,n int = 0,len(q)
    
    for n > 0 {
        sum = 0
        for i := 0; i < n; i++ {
            node = q[i]
            q[i] = nil
            sum += node.Val
            if node.Left != nil {
                q = append(q, node.Left)
            }
            if node.Right != nil {
                q = append(q, node.Right)
            }
        }
        ans = append(ans, float64(sum)/float64(n))
        q = q[n:]
        n = len(q)
    }
    return ans
}
```