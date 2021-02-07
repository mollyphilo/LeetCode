# Binary Tree Right Side View
Given a binary tree, imagine yourself standing on the _right_ side of it, return the values of the nodes you can see ordered from top to bottom.

**Example:**

	Input: [1,2,3,null,5,null,4]
	Output: [1, 3, 4]
	Explanation: 
	   1            <---
	 /   \
	2     3         <---
	 \     \
	  5     4       <---

**Solution:**

BST level order and the take rightmost value of each level

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func rightSideView(root *TreeNode) []int {
    if root == nil { return []int{} }
    q := []*TreeNode{root}
    ans := []int{}
    var subq []*TreeNode
    for len(q) > 0 {
        subq = []*TreeNode{}
        ans = append(ans, q[len(q)-1].Val)
        for len(q) > 0 {
            node := q[0]
            q[0] = nil
            q = q[1:]
            
            if node.Left != nil {
                subq = append(subq, node.Left)
            }
            if node.Right != nil {
                subq = append(subq, node.Right)
            }
        }
        q = subq
    }    
    return ans
}
```

Time and space Complexity: O(N)