# Two Sum IV - Input is a BST
Given the  `root`  of a Binary Search Tree and a target number  `k`, return  _`true`  if there exist two elements in the BST such that their sum is equal to the given target_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/21/sum_tree_1.jpg)

	Input: root = [5,3,6,2,4,null,7], k = 9
	Output: true

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/09/21/sum_tree_2.jpg)

	Input: root = [5,3,6,2,4,null,7], k = 28
	Output: false

**Example 3:**

	Input: root = [2,1,3], k = 4
	Output: true

**Example 4:**

	Input: root = [2,1,3], k = 1
	Output: false

**Example 5:**

	Input: root = [2,1,3], k = 3
	Output: true

**Constraints:**

-   The number of nodes in the tree is in the range  `[1, 104]`.
-   `-104 <= Node.val <= 104`
-   `root`  is guaranteed to be a  **valid**  binary search tree.
-   `-105 <= k <= 105`

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
func findTarget(root *TreeNode, k int) bool {
  // use any kind of traversal, and keep a map with visited values. If see k-node.val then we find a pair
  visited := make(map[int]struct{})
  return find_target(root, visited,k)
}

func find_target(root *TreeNode, visited map[int]struct{}, k int) bool {
  if root == nil { return false }
  if _,found := visited[k-root.Val]; found { return true }
  visited[root.Val] = struct{}{}
  
  if root.Left != nil && find_target(root.Left, visited, k) {
    return true
  }
  
  if root.Right != nil && find_target(root.Right, visited, k) {
    return true
  }
  return false
}
```