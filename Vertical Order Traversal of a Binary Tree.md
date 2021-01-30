# Vertical Order Traversal of a Binary Tree

Given the  `root`  of a binary tree, calculate the  **vertical order traversal**  of the binary tree.

For each node at position  `(row, col)`, its left and right children will be at positions  `(row + 1, col - 1)`  and  `(row + 1, col + 1)`  respectively. The root of the tree is at  `(0, 0)`.

The  **vertical order traversal**  of a binary tree is a list of top-to-bottom orderings for each column index starting from the leftmost column and ending on the rightmost column. There may be multiple nodes in the same row and same column. In such a case, sort these nodes by their values.

Return  _the  **vertical order traversal**  of the binary tree_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/29/vtree1.jpg)

	Input: root = [3,9,20,null,null,15,7]
	Output: [[9],[3,15],[20],[7]]
	Explanation:
	Column -1: Only node 9 is in this column.
	Column 0: Nodes 3 and 15 are in this column in that order from top to bottom.
	Column 1: Only node 20 is in this column.
	Column 2: Only node 7 is in this column.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/29/vtree2.jpg)

	Input: root = [1,2,3,4,5,6,7]
	Output: [[4],[2],[1,5,6],[3],[7]]
	Explanation:
	Column -2: Only node 4 is in this column.
	Column -1: Only node 2 is in this column.
	Column 0: Nodes 1, 5, and 6 are in this column.
	          1 is at the top, so it comes first.
	          5 and 6 are at the same position (2, 0), so we order them by their value, 5 before 6.
	Column 1: Only node 3 is in this column.
	Column 2: Only node 7 is in this column.

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/01/29/vtree3.jpg)

	Input: root = [1,2,3,4,6,5,7]
	Output: [[4],[2],[1,5,6],[3],[7]]

**Constraints:**

-   The number of nodes in the tree is in the range  `[1, 1000]`.
-   `0 <= Node.val <= 1000`

**Solution:**

Combination of DFS, map and Sort
```go
import (
    "sort"
)
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */

type Position struct {
    Node *TreeNode
    Row,Col int
}

type Stack []*Position

func (s Stack) Len() int { return len(s) }
func (s *Stack) Push(x *Position) { *s = append(*s, x) } 
func (s *Stack) Pop() *Position {
    old := *s
    n := len(old)
    item := old[n-1]
    old[n-1] = nil // avoid memory leak
    *s = old[0: n-1]
    return item
}
func (s Stack) Less(i,j int) bool {
    x,y := s[i],s[j]
    return (x.Row < y.Row) || (x.Row == y.Row && x.Node.Val < y.Node.Val)
}

func (s Stack) Swap(i,j int) { s[i],s[j] = s[j],s[i] }


func verticalTraversal(root *TreeNode) [][]int {
    // map of columes and its value
    cols := make(map[int]Stack)
    rootP := &Position{Node: root,Row: 0,Col:0}
    cols[0] = Stack{rootP}
    
    stack := new(Stack)
    stack.Push(rootP)
    
    var leftMostCol,rightMostCol int 
    
    for stack.Len() > 0 {
        node := stack.Pop()

        if node.Node.Left != nil {
            leftCol := node.Col-1
            leftPos := &Position{node.Node.Left,node.Row+1, leftCol}
            stack.Push(leftPos)
            cols[leftCol] = append(cols[leftCol], leftPos)
            if leftCol < leftMostCol {
                leftMostCol = leftCol
            }
        }
        if node.Node.Right != nil {
            rightCol := node.Col+1
            rightPos := &Position{node.Node.Right, node.Row+1, rightCol} 
            stack.Push(rightPos)
            cols[rightCol] = append(cols[rightCol], rightPos)
            if rightCol > rightMostCol {
                rightMostCol = rightCol
            }
        }
    }
    ans := [][]int{}
    for i := leftMostCol; i <= rightMostCol; i++ {
        sort.Sort(cols[i])
        n := len(cols[i])
        ansi := make([]int,n)
        for j := 0; j < n; j++ {
            ansi[j] = cols[i][j].Node.Val
        }
        ans = append(ans,ansi)
    }
    
    return ans
}
```