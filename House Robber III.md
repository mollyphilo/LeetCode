# House Robber III
The thief has found himself a new place for his thievery again. There is only one entrance to this area, called the "root." Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that "all houses in this place forms a binary tree". It will automatically contact the police if two directly-linked houses were broken into on the same night.

Determine the maximum amount of money the thief can rob tonight without alerting the police.

Example 1:

	Input: [3,2,3,null,3,null,1]

	     3
	    / \
	   2   3
	    \   \ 
	     3   1 
	Output: 7 
	Explanation: Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.

Example 2:

	Input: [3,4,5,1,3,null,1]
	     3
	    / \
	   4   5
	  / \   \ 
	 1   3   1

	Output: 9
	Explanation: Maximum amount of money the thief can rob = 4 + 5 = 9.

**Solution**

DP for rob or not rob option.

Time Complexity = O(2*N) = O(N), where N is the size of the tree

Space Complexity = O(4*N) = O(N), same N as above

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func rob(root *TreeNode) int {
    if root == nil {
        return 0
    }
    queue := []*TreeNode{root}
    indexes := []int{0}
    values := []int{root.Val}
    childrenIndexes := make(map[int][]int)
    
    i := 0
    for len(queue) > 0 {
        n := len(queue)
        node := queue[n-1]
        queue[n-1] = nil
        queue = queue[:n-1]
        
        parentIndex := indexes[n-1]
        indexes = indexes[:n-1]
        
        if node.Left != nil {
            queue = append(queue,node.Left)
            values = append(values, node.Left.Val)
            i += 1
            indexes = append(indexes,i)
            childrenIndexes[parentIndex] = append(childrenIndexes[parentIndex], i)
        }
        if node.Right != nil {
            queue = append(queue,node.Right)
            values = append(values, node.Right.Val)
            i += 1
            indexes = append(indexes,i)
            childrenIndexes[parentIndex] = append(childrenIndexes[parentIndex], i)
        }
    }

    dp_rob := make([]int, i+1)
    dp_not_rob := make([]int, i+1)
    for j := i; j >= 0; j-- {
        dp_rob[j] = values[j]
        dp_not_rob[j] = 0
        for _,cj := range childrenIndexes[j] {
            dp_rob[j] += dp_not_rob[cj]
            dp_not_rob[j] += max(dp_rob[cj],dp_not_rob[cj])
        }
    }
    
    return max(dp_rob[0],dp_not_rob[0])
}

func max(a,b int) int {
    if a > b { return a }
    return b
}
```