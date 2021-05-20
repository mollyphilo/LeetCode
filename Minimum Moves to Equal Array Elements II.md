# Minimum Moves to Equal Array Elements II

Given an integer array  `nums`  of size  `n`, return  _the minimum number of moves required to make all array elements equal_.

In one move, you can increment or decrement an element of the array by  `1`.

**Example 1:**

	Input: nums = [1,2,3]
	Output: 2
	Explanation:
	Only two moves are needed (remember each move increments or decrements one element):
	[1,2,3]  =>  [2,2,3]  =>  [2,2,2]

**Example 2:**

	Input: nums = [1,10,2,9]
	Output: 16

**Constraints:**

-   `n == nums.length`
-   `1 <= nums.length <= 105`
-   `-109  <= nums[i] <= 109`

**Solution:**

```go
// use quickselect to find median, or the (n/2)th + 1 number in the array. Note that it is median, not average, to reduce the distance to the 2 ends
func minMoves2(nums []int) int {
    n := len(nums)
    median := findKthElement(nums,0,n-1,n/2 + 1)
    
    var moves int
    for _,num := range nums {
        if median < num {
            moves += num-median
        }else {
            moves += median-num
        }
    }
    return moves
}

func findKthElement(nums []int, left,right int, k int) int {
    l,r,pivot := left,right,nums[(left+right)/2]
    for l <= r {
        for nums[l] < pivot { l++ }
        for nums[r] > pivot { r-- }
        if l >= r { break }
        temp := nums[l]
        nums[l] = nums[r]
        nums[r] = temp
        l++
        r--
    }
    if l-left+1 > k { return findKthElement(nums,left,l-1,k) }
    if l-left+1 == k && l == r { return nums[l] }
    return findKthElement(nums,r+1,right,left+k-1-r)
}
```