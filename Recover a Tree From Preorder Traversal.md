# Recover a Tree From Preorder Traversal

We run a preorder depth-first search (DFS) on the  `root`  of a binary tree.

At each node in this traversal, we output  `D`  dashes (where  `D`  is the depth of this node), then we output the value of this node. If the depth of a node is  `D`, the depth of its immediate child is  `D + 1`. The depth of the  `root`  node is  `0`.

If a node has only one child, that child is guaranteed to be  **the left child**.

Given the output  `S`  of this traversal, recover the tree and return  _its_  `root`.

**Example 1:**

    ![](https://assets.leetcode.com/uploads/2019/04/08/recover-a-tree-from-preorder-traversal.png)

    Input: S = "1-2--3--4-5--6--7"
    Output: [1,2,5,3,4,6,7]

**Example 2:**

    ![](https://assets.leetcode.com/uploads/2019/04/11/screen-shot-2019-04-10-at-114101-pm.png)

    Input: S = "1-2--3---4-5--6---7"
    Output: [1,2,5,3,null,6,null,4,null,7]

**Example 3:**

    ![](https://assets.leetcode.com/uploads/2019/04/11/screen-shot-2019-04-10-at-114955-pm.png)

    Input: S = "1-401--349---90--88"
    Output: [1,401,null,349,88,90]

**Constraints:**

-   The number of nodes in the original tree is in the range  `[1, 1000]`.
-   `1 <= Node.val <= 109`

**Solution:**

```go
import (
    "strconv"
)
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func recoverFromPreorder(S string) *TreeNode {
    var level,n int = 0,len(S)
    var i int

    for i = 0; i < n &&  S[i] != '-'; i++{}
    num,_ := strconv.Atoi(S[0:i])
    root := &TreeNode{Val: num}
    nodes := [][]*TreeNode{{root}}
    
    var v *TreeNode

    for i < n {
        if S[i] == '-' {
            for ;i < n && S[i] == '-'; i++{
                level++
            }
        }else {
            j := i
            for ; i < n &&  S[i] != '-'; i++{}
            num,_ := strconv.Atoi(S[j:i])
            v = &TreeNode{Val: num}
            if nodes[level-1][len(nodes[level-1])-1].Left == nil {
                nodes[level-1][len(nodes[level-1])-1].Left = v
            }else {
                nodes[level-1][len(nodes[level-1])-1].Right = v
            }
            
            if len(nodes) == level {
                nodes = append(nodes, []*TreeNode{v})
            }else {
                nodes[level] = append(nodes[level],v)
            }
            
            level = 0
        }
    }

    return nodes[0][0]
}
```
