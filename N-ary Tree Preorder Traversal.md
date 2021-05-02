# N-ary Tree Preorder Traversal
Given the  `root`  of an n-ary tree, return  _the preorder traversal of its nodes' values_.

Nary-Tree input serialization is represented in their level order traversal. Each group of children is separated by the null value (See examples)

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/12/narytreeexample.png)

    Input: root = [1,null,3,2,4,null,5,6]
    Output: [1,3,5,6,2,4]

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/11/08/sample_4_964.png)

    Input: root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
    Output: [1,2,3,6,7,11,14,4,8,12,5,9,13,10]

**Constraints:**

-   The number of nodes in the tree is in the range  `[0, 104]`.
-   `0 <= Node.val <= 104`
-   The height of the n-ary tree is less than or equal to  `1000`.

**Follow up:**  Recursive solution is trivial, could you do it iteratively?

---

**Solution:**

Append children from rightmost to leftmost to stack so that the leftmost and its children are pop first

```go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Children []*Node
 * }
 */

func preorder(root *Node) []int {
    if root == nil {
        return make([]int,0)
    }
    stack := []*Node{root}
    var ans []int
    
    var n int = 1
    var node *Node
    for n > 0 {
        node = stack[n-1]
        stack[n-1] = nil
        stack = stack[0:n-1]
        ans = append(ans,node.Val)
        for i := len(node.Children)-1; i >= 0; i-- {
            stack = append(stack, node.Children[i])
        }
        n = len(stack)
    }
    return ans
}
```

---

**BONUS**

# N-ary Tree Postorder Traversal

Also use stack, but append chidren from left to right, and prepend values to ans array

```go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Children []*Node
 * }
 */

func postorder(root *Node) []int {
    if root == nil {
        return make([]int,0)
    }
    
    stack := []*Node{root}
    
    var ans []int
    n := 1
    var node *Node
    for n > 0 {
        node = stack[n-1]
        stack[n-1] = nil
        stack = stack[0:n-1]
        ans = append( ans, node.Val)
        
        for i := 0; i < len(node.Children); i++ {
            stack = append(stack, node.Children[i])
        }
        
        n = len(stack)
    }
    
    n = len(ans)
    result := make([]int, n)
    for i := 0; i < n; i++ {
        result[i] = ans[n-1-i]
    }
    return result
}
```

