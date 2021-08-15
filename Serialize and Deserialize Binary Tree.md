# Serialize and Deserialize Binary Tree
Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

**Clarification:**  The input/output format is the same as  [how LeetCode serializes a binary tree](https://leetcode.com/faq/#binary-tree). You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/15/serdeser.jpg)

    Input: root = [1,2,3,null,null,4,5]
    Output: [1,2,3,null,null,4,5]

**Example 2:**

    Input: root = []
    Output: []

**Example 3:**

    Input: root = [1]
    Output: [1]

**Example 4:**

    Input: root = [1,2]
    Output: [1,2]

**Constraints:**

-   The number of nodes in the tree is in the range  `[0, 104]`.
-   `-1000 <= Node.val <= 1000`

**Solution:**

Approach 1: Level order traversal

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */

import "strconv"

const (
    separator = ","
    null = "null"
)

type Codec struct {
    
}

func Constructor() Codec {
    return Codec{}    
}

// Serializes a tree to a single string.
func (this *Codec) serialize(root *TreeNode) string {
    if root == nil { return null }
    
    queue := []*TreeNode{root}
    n := len(queue)
    var node *TreeNode
    var ans []string
    
    for n > 0 {
        for i := 0; i < n; i++ {
            node = queue[i]
            if node == nil {
                ans = append(ans,null)
                continue
            }
            ans = append(ans,strconv.Itoa(node.Val))
            queue = append(queue, node.Left)
            queue = append(queue, node.Right)    
        }
        queue = queue[n:]
        n = len(queue)
    }
    return strings.Join(ans,separator)
}

// Deserializes your encoded data to tree.
func (this *Codec) deserialize(data string) *TreeNode {    
    arr := strings.Split(data,separator)
    if len(arr) == 0 || arr[0] == null { return nil }
    
    v,_ := strconv.Atoi(arr[0])
    root := &TreeNode{Val: v}
    queue := []*TreeNode{root}
    k,n := 1,len(queue)
    var node,left,right *TreeNode
    
    for n > 0 {
        for i := 0; i < n; i++ {
            node = queue[i]
            if node != nil {
                left,right = nil,nil
                if arr[k] != null {
                    v,_ = strconv.Atoi(arr[k])
                    left = &TreeNode{Val: v}
                }
                k++
                if arr[k] != null {
                    v,_ = strconv.Atoi(arr[k])
                    right = &TreeNode{Val: v}
                }
                node.Left,node.Right = left,right
                queue = append(queue,left,right)
                k++
            }
            
        }
        queue = queue[n:]
        n = len(queue)
    }
    
    return root
}


/**
 * Your Codec object will be instantiated and called as such:
 * ser := Constructor();
 * deser := Constructor();
 * data := ser.serialize(root);
 * ans := deser.deserialize(data);
 */
```

Approach 2: Pre-order traversal

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
import "strconv"

const (
    separator = ","
    null = "null"
)

type Codec struct {
    
}

func Constructor() Codec {
    return Codec{}
}

// Serializes a tree to a single string.
func (this *Codec) serialize(root *TreeNode) string {
    if root == nil { return null }
    stack := []*TreeNode{root}
    n := len(stack)
    var node *TreeNode
    var b strings.Builder
    
    for n > 0 {
        node = stack[n-1]
        stack[n-1] = nil
        stack = stack[:n-1]
        
        if node == nil {
            b.WriteString(null)
        }else {
            b.WriteString(strconv.Itoa(node.Val))    
            stack = append(stack,node.Right)
            stack = append(stack,node.Left)
        }
        b.WriteRune(',')
        n = len(stack)
    }
    
    ans := b.String()
    fmt.Println(ans)
    return ans[0:len(ans)-1]
}

// Deserializes your encoded data to tree.
func (this *Codec) deserialize(data string) *TreeNode {    
    vals := strings.Split(data,separator)
    if len(vals) == 0 { return nil }
    return helper(vals,[]int{0})
}

func helper(vals []string, i []int) *TreeNode {
    if vals[i[0]] == null { return nil }
    v,_ := strconv.Atoi(vals[i[0]])
    root := &TreeNode{Val: v}
    i[0] += 1
    root.Left = helper(vals,i)
    i[0] += 1
    root.Right = helper(vals,i)
    
    return root
}
```

Still pre-order traversal, but not using stack

```go
import (
  "fmt"
  "strings"
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
const (
  delimiter string = ","
  nullNode string = "null"
)

type Codec struct {
    builder strings.Builder
}

func Constructor() Codec {
  return Codec{}    
}

// Serializes a tree to a single string.
func (this *Codec) serialize(root *TreeNode) string {
  // with preorder traversal
  this.builder.Reset()
  
  this.buildString(root)
  ans := this.builder.String()
  
  this.builder.Reset()
  return ans
}

func (this *Codec) buildString(root *TreeNode) {
  if root == nil { this.builder.WriteString(nullNode); return }
  fmt.Fprintf(&this.builder, "%d,", root.Val)
  this.buildString(root.Left)
  this.builder.WriteString(delimiter)
  this.buildString(root.Right)
}

// Deserializes your encoded data to tree.
func (this *Codec) deserialize(data string) *TreeNode {
  if len(data) == 0 { return nil }
  arr := strings.Split(data, delimiter)
  
  var idx int
  return buildNode(arr, &idx)
}


func buildNode(arr []string, i *int) *TreeNode {
  if arr[*i] == nullNode { return nil }
  rootVal,_ := strconv.Atoi(arr[*i])
  root := &TreeNode{Val: rootVal}
  (*i)++
  root.Left = buildNode(arr, i)
  (*i)++
  root.Right = buildNode(arr, i)
  
  return root
}

/**
 * Your Codec object will be instantiated and called as such:
 * ser := Constructor();
 * deser := Constructor();
 * data := ser.serialize(root);
 * ans := deser.deserialize(data);
 */
```

## Bonus: N-ary Tree

Given a Generic Tree, Serialize and Deserialize it.
Serialization is a basically a representation of a tree in a String format which takes much lesser space than storing the tree itself.
Deserialization is constructing the actual tree using the the serialized format of the tree.

Serialization DeSerialization Tree

```
            1
        /   |   \
    2       3       4
  /   \           /   \
5       6       7       8
                        |
                        9
```
OUTPUT:

Serialized Tree :
1 2 5 ) 6 ) ) 3 ) 4 7 ) 8 9 ) ) ) )

Deserialized Tree :
1 => [ 2 3 4 ] 2 => [ 5 6 ] 5 => [ ] 6 => [ ] 3 => [ ] 4 => [ 7 8 ] 7 => [ ] 8 => [ 9 ] 9 => [ ]


```go
package main

import (
	"fmt"
	"strings"
	"strconv"
	"reflect"
)

const (
  delimiter string = ","
  nullNode string = "null"
)

type TreeNode struct {
	Val int
	Children []*TreeNode
}

type Codec struct {
	builder strings.Builder
}

func Constructor() Codec {
  return Codec{}    
}

func (this *Codec) Serialize(root *TreeNode) string {
	this.builder.Reset()
	this.buildString(root)
	defer this.builder.Reset()
	
	return this.builder.String()
}

func (this *Codec) buildString(root *TreeNode) {
	if root == nil { this.builder.WriteString(nullNode); return }
	fmt.Fprintf(&this.builder, "%d,", root.Val)
	
	n := len(root.Children)
	if n == 0 { this.builder.WriteRune('#'); return }
	
	for i := 0; i < n; i++ {
		this.buildString(root.Children[i])
		this.builder.WriteRune(',')
	}
	this.builder.WriteRune('#')
}

func (this *Codec) Deserialize(data string) *TreeNode {
	if len(data) == 0 { return nil }
	arr := strings.Split(data, ",")
	var idx int
	return this.buildNode(arr, &idx)
}

func (this *Codec) buildNode(arr []string, i *int) *TreeNode {
	if arr[*i] == nullNode { return nil }
	rootVal,_ := strconv.Atoi(arr[*i])
	root := &TreeNode{Val: rootVal}
	(*i)++
	for (*i) < len(arr) && arr[*i] != "#" {
		root.Children = append(root.Children, this.buildNode(arr, i))
		(*i)++	
	}
	return root

}

func main() {
	root := &TreeNode{Val: 1}
	root.Children = make([]*TreeNode,3)

	node1 := &TreeNode{Val: 2}
	node1.Children = make([]*TreeNode, 2)
	node1.Children[0] = &TreeNode{Val: 5}
	node1.Children[1] = &TreeNode{Val: 6}
	root.Children[0] = node1
	
	root.Children[1] = &TreeNode{Val: 3}
	
	node2 := &TreeNode{Val: 4}
	node2.Children = make([]*TreeNode, 2)
	node2.Children[0] = &TreeNode{Val: 7}
	node2.Children[1] = &TreeNode{Val: 8, Children: []*TreeNode{&TreeNode{Val:9}}}
	root.Children[2] = node2
	
	codec := Constructor()
	data := codec.Serialize(root)
	fmt.Println("before: ",data)
	
	decodedRoot := codec.Deserialize(data)
	decodedData := codec.Serialize(decodedRoot)
	fmt.Println("after: ", decodedData)
	fmt.Printf("before == after ? %v\n",reflect.DeepEqual(data,decodedData))
	
}

/*
Runing main.go will give
-------------------------
before:  1,2,5,#,6,#,#,3,#,4,7,#,8,9,#,#,#,#
after:   1,2,5,#,6,#,#,3,#,4,7,#,8,9,#,#,#,#
before == after ? true
*/
```

