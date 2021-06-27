# Count of Smaller Numbers After Self
You are given an integer array  `nums`  and you have to return a new  `counts`  array. The  `counts`  array has the property where  `counts[i]`  is the number of smaller elements to the right of  `nums[i]`.

**Example 1:**

	Input: nums = [5,2,6,1]
	Output: [2,1,1,0]
	Explanation:
	To the right of 5 there are **2** smaller elements (2 and 1).
	To the right of 2 there is only **1** smaller element (1).
	To the right of 6 there is **1** smaller element (1).
	To the right of 1 there is **0** smaller element.

**Example 2:**

	Input: nums = [-1]
	Output: [0]

**Example 3:**

	Input: nums = [-1,-1]
	Output: [0,0]

**Constraints:**

-   `1 <= nums.length <= 105`
-   `-104  <= nums[i] <= 104`

**Solution:**

Binary Search Tree: Time Limit Exceeded

Time Complexity: N * O(H) (height of the tree) or O(NxH). Space complexity: O(N)

```go
// BST
type Node struct {
    Val,Equals,Smallers int
    Left,Right *Node
}
func countSmaller(nums []int) []int {
    n := len(nums)
    counts := make([]int, n)
    root := &Node{Val: nums[n-1], Equals: 1}
    for i := n-2; i >= 0; i-- {
        counts[i] = insert(root, nums[i])
    }
    
    return counts
}

func insert(node *Node, val int) int {
    if val < node.Val {
        node.Smallers++
        if node.Left == nil {
            node.Left = &Node{Val: val, Equals: 1}
            return 0
        }
        return insert(node.Left,val)
    }else if val > node.Val {
        if node.Right == nil {
            node.Right = &Node{Val: val, Equals: 1}
            return node.Smallers + node.Equals
        }
        return node.Smallers + node.Equals + insert(node.Right, val)
    }else {
        node.Equals++
        return node.Smallers
    }
}
```

Binary Index Tree or Fenwick tree: Time Complexity: O(N) because update/query sum is O(logN), but preprocessing is O(N). Space complexity: O(N)

```go
import "sort"

type BIT []int

func (b BIT) update(index,val int) {
    for i := index+1; i < len(b); i += i&(-i) {
        b[i] += val
    }
}

func (b BIT) query(index int) int {
    var sum int
    for i := index + 1; i > 0; i -= i&(-i) {
        sum += b[i]
    }
    return sum
}

func countSmaller(nums []int) []int {
    n := len(nums)
    
    sorted := make([]int,n)
    copy(sorted,nums)
    sort.Ints(sorted)
    
    n2i := make(map[int]int)
    var idx int
    for i := range sorted {
        if _,ok := n2i[sorted[i]]; !ok {
            n2i[sorted[i]] = idx
            idx++
        }
    }
    
    bit := make(BIT, len(n2i)+1)
    
    counts := make([]int, n)
    for i := n-1; i >= 0; i-- {
        counts[i] = bit.query(n2i[nums[i]]-1)
        bit.update(n2i[nums[i]], 1)
    }
    return counts
}
```