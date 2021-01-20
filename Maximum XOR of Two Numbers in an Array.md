# Maximum XOR of Two Numbers in an Array
Given an integer array  `nums`, return  _the maximum result of  `nums[i] XOR nums[j]`_, where  `0 ≤ i ≤ j < n`.

**Follow up:**  Could you do this in  `O(n)`  runtime?

**Example 1:**

    Input: nums = [3,10,5,25,2,8]
    Output: 28
    Explanation: The maximum result is 5 XOR 25 = 28.

**Example 2:**

    Input: nums = [0]
    Output: 0

**Example 3:**

	Input: nums = [2,4]
	Output: 6

**Example 4:**

	Input: nums = [8,10,2]
	Output: 10

**Example 5:**

	Input: nums = [14,70,53,83,49,91,36,80,92,51,66,70]
	Output: 127

**Constraints:**

-   `1 <= nums.length <= 2 * 104`
-   `0 <= nums[i] <= 231  - 1`

**Solutions:**
#### Approach 1: Bit Masking
Let max be 0. We sequentially set its bits to 1, from the most significant bit. For each, we check if there is any pair in the array that sum up to current max. 
You may think that look up will take O(N^2), with N is the size of the array. However, we use XOR operation to do the trick in O(N)
```
a^b = c and a^b^b = a
-> a = a^b^b = c^b
```
c is our current max, b is every number we loop in our array. So we can find the other number of the pair by doing c^b

Another thing we need to do before searching for the pair is clearing all the bits of the number in the arrays so that only their left most to ith bits are set, in the same fashion of our current max. Why doing so? Because we would not be able to find a pair matches up to current max if their ith to right most bits are set.

```go
func findMaximumXOR(nums []int) int {
    mask,currentMax,max := 0,0,0
    set := make(map[int]struct{})
    
    for i := 30; i >= 0; i-- {
        mask |= (1 << i)
        for _,v := range nums {
            set[(mask & v)] = struct{}{}
        }
        currentMax = max | (1 << i)
        for k := range set {
            v := (currentMax ^ k)
            if _, ok := set[v]; ok {
                max = currentMax
                break
            }
        }
        set = make(map[int]struct{})
    }
    
    return max
}
```
Time complexity: O(N*log M) where N is size of the array and M is total unique numbers in the array

#### Approach 2: Trie Node Bit Masking
Build a trie with each node has children 0 and/or 1. The whole trie represents all the binary options of our numbers. For every number in the array, we pair its bits (from the most significant one) with its opposite if one branches on the trie. This gives us the maximum of this number when XOR with any number in the array. After looping through all the number in the array, we have our max

```go
type TrieNode struct {
    children [2]*TrieNode
}

func NewTrieNode() *TrieNode {
    return &TrieNode{children: [2]*TrieNode{}}
}

func (t *TrieNode) get(i int) *TrieNode {
    return t.children[i]
}

func (t *TrieNode) add(i int) {
    t.children[i] = NewTrieNode()
}

func findMaximumXOR(nums []int) int {
    // build trie 
    root := NewTrieNode()
    var node *TrieNode
    var bit int
    for _,n := range nums {
        node = root
        for i := 31; i >=0; i-- {
            bit = (n >> i) & 1
            if node.get(bit) == nil {
                node.add(bit)
            }
            node = node.get(bit)
        }
    }
    
    // travel trie to find best XOR result
    var max,currentMax int
    for _,n := range nums {
        node = root
        currentMax = 0
        for i := 31; i >=0; i-- {
            bit = (n >> i) & 1
            if node.get(bit^1) != nil {
                currentMax |= (1 << i)
                node = node.get(bit^1)
            }else {
                node = node.get(bit)     
            }
        }
        if currentMax > max {
            max = currentMax
        }
    }
    
    return max
}
```