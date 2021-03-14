# Add One Row to Tree
Given the  `root`  of a binary tree and two integers  `val`  and  `depth`, add a row of nodes with value  `val`  at the given depth  `depth`.

Note that the  `root`  node is at depth  `1`.

The adding rule is:

-   Given the integer  `depth`, for each not null tree node  `cur`  at the depth  `depth - 1`, create two tree nodes with value  `val`  as  `cur`'s left subtree root and right subtree root.
-   `cur`'s original left subtree should be the left subtree of the new left subtree root.
-   `cur`'s original right subtree should be the right subtree of the new right subtree root.
-   If  `depth == 1`  that means there is no depth  `depth - 1`  at all, then create a tree node with value  `val`  as the new root of the whole original tree, and the original tree is the new root's left subtree.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/11/add1-tree.jpg)

**Input:** root = [4,2,6,3,1,5], val = 1, depth = 2
**Output:** [4,1,1,2,null,null,6,3,1,5]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/11/add2-tree.jpg)

**Input:** root = [4,2,null,3,1], val = 1, depth = 3
**Output:** [4,2,null,1,1,3,null,null,1]

**Constraints:**

-   The number of nodes in the tree is in the range  `[1, 104]`.
-   The depth of the tree is in the range  `[1, 104]`.
-   `-100 <= Node.val <= 100`
-   `-105  <= val <= 105`
-   `1 <= depth <= the depth of tree + 1`

**Solution:**

BFS for level traversal

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func addOneRow(root *TreeNode, val int, depth int) *TreeNode {
    if depth == 1 {
        return &TreeNode{Val: val, Left: root}
    }
    queue := []*TreeNode{root}
    n, curDepth := len(queue),1

    for n > 0 {
        curDepth += 1

        if curDepth == depth {
            fmt.Println(curDepth)
            for i := 0; i < n; i++ {
                if node := queue[i]; node != nil {
                    left,right := &TreeNode{Val: val},&TreeNode{Val: val}
                    oldLeft,oldRight := node.Left,node.Right
                    node.Left = left
                    left.Left = oldLeft

                    node.Right = right
                    right.Right = oldRight
                }
            }        
            return root
        }
        for i := 0; i < n; i++ {
            if node := queue[i]; node != nil {
                queue = append(queue, node.Left)
                queue = append(queue, node.Right)   
            }         
        }
        queue = queue[n:]
        n = len(queue)
    }
    return nil
}
```