# Binary Tree Cameras

Given a binary tree, we install cameras on the nodes of the tree.

Each camera at a node can monitor  **its parent, itself, and its immediate children**.

Calculate the minimum number of cameras needed to monitor all nodes of the tree.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/29/bst_cameras_01.png)

	Input: [0,0,null,0,0]
	Output: 1
	Explanation: One camera is enough to monitor all nodes if placed as shown.

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/29/bst_cameras_02.png)

	Input: [0,0,null,0,null,0,null,null,0]
	Output: 2
	Explanation: At least two cameras are needed to monitor all nodes of the tree. The above image shows one of the valid configurations of camera placement.

  
**Note:**

1.  The number of nodes in the given tree will be in the range `[1, 1000]`.
2.  **Every**  node has value 0.

**Solution:**

Greedy:  Let say we have 3 status
- Monitored with cam on the node
- Monitored wihout cam on the node - cam is installed in the neighbors
- Not monitored

Therefore we have these monitoring cases:
- If one of the children is not monitored, parent needs a cam
- If both children are monitored with cam, parent is monitored no cam
- If both chidren are monitored with no cam, parent is not monitored
- Default: monitored with no cam

```go
const (
    NOT_MONITORED = iota
    MONITORED_NOCAM
    MONITORED_WITHCAM
)
func minCameraCover(root *TreeNode) int {
    if root == nil { return 0 }
    var cams int
    if status := dfs(root,&cams); status == NOT_MONITORED {
        cams++
    }
    return cams
}

func dfs(root *TreeNode, cameras *int) int {
    if root == nil { return MONITORED_NOCAM }
    left := dfs(root.Left,cameras)
    right := dfs(root.Right,cameras)
    
    if left == NOT_MONITORED || right == NOT_MONITORED {
        *cameras += 1
        return MONITORED_WITHCAM
    }else if left == MONITORED_NOCAM && right == MONITORED_NOCAM {
        return NOT_MONITORED
    }else {
        return MONITORED_NOCAM
    }
}
```