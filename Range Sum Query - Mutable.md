# Range Sum Query - Mutable
## Binary Indexed Tree
Used to solve Range Sum Query, where naive solution of summing all elements takes O(N) time complexity and be too slow for too big an array
**Basic idea:**
A number can be represented as sum of power of 2s (e.g. 5 = 2^2 + 2^1). A sum of a range can be represented as sum of subsequent ranges, e.g. sum[1->12] = sum[9->12] + sum[1->8]
**Fenwick tree or BIT:**
Consider BIT as 1-based array.
Given original array nums
```
			{
BIT[i] = 		nums[1...i] if x is a power of 2,else
				nums[j...i] for j is the element since the preceding power of 2
			}
```
Each element in BIT cotains sum of values since its parent in the tree, and the parent can be found by clearing the least significant bit in its index.

To query sum of any given BIT index, consider binary expand of the index, and add value which correspond to BIT value at the expansion. For example:
```
BIT[1101] = BIT[1101] + BIT[1100] + BIT[1000]
```
To update BIT at i position, BIT[i] needs to be updated as well as its parents, which can be found by adding 1 to its last set bit
```
update BIT[1101]
update BIT[1110]
update BIT[10000]
```

**How to find last set bit:**
x&-x
Proof:
x = a1b, where b contains all 0s so that 1 is the last set bit, and a contains any number of 1s and 0s
-x is 2's complement of x = (x inverted) + 1 = x' + 1
-x = x' + 1 = (a1b)' + 1 = a'0(0..0)' + 1 = a'0(1..1) + 1 = a'1(0..0) = a'1b
x & -x = a1b & a'1b = (0..0)1(0..0)

## Problem
Given an array  `nums`  and two types of queries where you should update the value of an index in the array, and retrieve the sum of a range in the array.

Implement the  `NumArray`  class:

-   `NumArray(int[] nums)`  Initializes the object with the integer array  `nums`.
-   `void update(int index, int val)`  Updates the value of  `nums[index]`  to be  `val`.
-   `int sumRange(int left, int right)`  Returns the sum of the subarray  `nums[left, right]`  (i.e.,  `nums[left] + nums[left + 1], ..., nums[right]`).

**Example 1:**

    Input
    ["NumArray", "sumRange", "update", "sumRange"]
    [[[1, 3, 5]], [0, 2], [1, 2], [0, 2]]
	
	Output
    [null, 9, null, 8]
    
    Explanation
    NumArray numArray = new NumArray([1, 3, 5]);
    numArray.sumRange(0, 2); // return 9 = sum([1,3,5])
    numArray.update(1, 2);   // nums = [1,2,5]
    numArray.sumRange(0, 2); // return 8 = sum([1,2,5])

**Solution**

```go
type NumArray struct {
    nums []int
    BIT []int
}

func Constructor(nums []int) NumArray {
    n := len(nums)
    bits := make([]int, n+1)
    
    for i := 0; i < n; i++ {
        val := nums[i]
        j := i+1 // because BIT is 1-based array
        for j <= n {
            bits[j] += val
            j += (j&-j)
        }
        
    }

    return NumArray{
        nums: nums,
        BIT: bits,
    }
}

func (this *NumArray) Update(index int, val int)  {
    diff := val - this.nums[index]
    this.nums[index] = val
    index++ // because BIT is 1-based array
    for index < len(this.BIT) {
        this.BIT[index] += diff
        index += (index&-index)
    }
}

func (this *NumArray) getSum(index int) int {
    index++ // because BIT is 1-based array
    sum := 0
    for index > 0 {
        sum += this.BIT[index]
        index -= (index&-index)
    }
    return sum
}

func (this *NumArray) SumRange(left int, right int) int {
    if left == 0 {
        return this.getSum(right)
    }
    return this.getSum(right) - this.getSum(left-1) // remove 1 from left to include value at index left to the result
}

/**
 * Your NumArray object will be instantiated and called as such:
 * obj := Constructor(nums);
 * obj.Update(index,val);
 * param_2 := obj.SumRange(left,right);
 */
```