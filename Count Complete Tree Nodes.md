# 222.  Count Complete Tree Nodes

Given the  `root`  of a  **complete**  binary tree, return the number of the nodes in the tree.

According to  **[Wikipedia](http://en.wikipedia.org/wiki/Binary_tree#Types_of_binary_trees)**, every level, except possibly the last, is completely filled in a complete binary tree, and all nodes in the last level are as far left as possible. It can have between  `1`  and  `2h`  nodes inclusive at the last level  `h`.

Design an algorithm that runs in less than `O(n)` time complexity.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/14/complete.jpg)

	Input: root = [1,2,3,4,5,6]
	Output: 6

**Example 2:**

	Input: root = []
	Output: 0

**Example 3:**

	Input: root = [1]
	Output: 1

**Constraints:**

-   The number of nodes in the tree is in the range  `[0, 5 * 104]`.
-   `0 <= Node.val <= 5 * 104`
-   The tree is guaranteed to be  **complete**.

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
func countNodes(root *TreeNode) int {
    if root == nil { return 0 }
    if root.Left == nil && root.Right == nil { return 1 }
    
    levels := getLevels(root)
    
    return getRightMostAtLevel(root,levels, 1, 1)
}

func getLevels(root *TreeNode) int {
    var level int
    for root != nil {
        level++
        root = root.Left
    }
    
    return level
}

func getRightMostAtLevel(root *TreeNode, levels, currLevel, counter int) int {
    if root == nil { return -1 }
    if currLevel == levels {
        return counter
    }
    
    count := getRightMostAtLevel(root.Right, levels, currLevel + 1, counter * 2 + 1)
    if count != -1 { return count }
    return getRightMostAtLevel(root.Left, levels, currLevel + 1, counter * 2)
}
```