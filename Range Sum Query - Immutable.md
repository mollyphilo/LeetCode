# Range Sum Query - Immutable

Given an integer array  `nums`, handle multiple queries of the following type:

1.  Calculate the  **sum**  of the elements of  `nums`  between indices  `left`  and  `right`  **inclusive**  where  `left <= right`.

Implement the  `NumArray`  class:

-   `NumArray(int[] nums)`  Initializes the object with the integer array  `nums`.
-   `int sumRange(int left, int right)`  Returns the  **sum**  of the elements of  `nums`  between indices  `left`  and  `right`  **inclusive**  (i.e.  `nums[left] + nums[left + 1] + ... + nums[right]`).

**Example 1:**

	Input
	["NumArray", "sumRange", "sumRange", "sumRange"]
	[[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]
	Output
	[null, 1, -1, -3]

**Explanation**
	NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
	numArray.sumRange(0, 2); // return (-2) + 0 + 3 = 1
	numArray.sumRange(2, 5); // return 3 + (-5) + 2 + (-1) = -1
	numArray.sumRange(0, 5); // return (-2) + 0 + 3 + (-5) + 2 + (-1) = -3

**Constraints:**

-   `1 <= nums.length <= 104`
-   `-105  <= nums[i] <= 105`
-   `0 <= left <= right < nums.length`
-   At most  `104`  calls will be made to  `sumRange`.

**Solution:**

BITs

```go
type NumArray struct {
    bits []int
}


func Constructor(nums []int) NumArray {
  n := len(nums)
  bits := make([]int, n+1)
  for i := 0; i < n; i++ {
    val := nums[i]
    j := i+1 // bits is 1-based index
    for j <= n {
      bits[j] += val
      j += (j&-j)
    }
  }
  return NumArray{bits}
}

func (this *NumArray) SumRange(left int, right int) int {
  ans := this.getSum(right)
  if left == 0 { return ans }
  return ans - this.getSum(left-1) 
}

func (this *NumArray) getSum(i int) int {
  var ans int
  i++
  for i > 0 {
    ans += this.bits[i]
    i -= (i&-i)
  }
  return ans
}


/**
 * Your NumArray object will be instantiated and called as such:
 * obj := Constructor(nums);
 * param_1 := obj.SumRange(left,right);
 */
```

Segment tree

```go
type NumArray struct {
  segmentTree []int
  n int
}


func Constructor(nums []int) NumArray {
  n := len(nums)
  segment := make([]int, n+n)
  copy(segment[n:], nums)
  for i := n-1; i > 0; i-- {
    segment[i] = segment[i<<1] + segment[i<<1 + 1]
  }
  
  return NumArray{segment,n}
}


func (this *NumArray) SumRange(left int, right int) int {
  left += this.n
  right += this.n
  var sum int
  for left <= right {
    // left is right child of its parent
    if left % 2 == 1 {
      sum += this.segmentTree[left]
      left++
    }
    
    if right % 2 == 0 {
      sum += this.segmentTree[right]
      right--
    }
    left >>= 1
    right >>=1
  }
  
  return sum
}


/**
 * Your NumArray object will be instantiated and called as such:
 * obj := Constructor(nums);
 * param_1 := obj.SumRange(left,right);
 */
```