# 154.  Find Minimum in Rotated Sorted Array II

Suppose an array of length  `n`  sorted in ascending order is  **rotated**  between  `1`  and  `n`  times. For example, the array  `nums = [0,1,4,4,5,6,7]`  might become:

-   `[4,5,6,7,0,1,4]`  if it was rotated  `4`  times.
-   `[0,1,4,4,5,6,7]`  if it was rotated  `7`  times.

Notice that  **rotating**  an array  `[a[0], a[1], a[2], ..., a[n-1]]`  1 time results in the array  `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`.

Given the sorted rotated array  `nums`  that may contain  **duplicates**, return  _the minimum element of this array_.

You must decrease the overall operation steps as much as possible.

**Example 1:**

	Input: nums = [1,3,5]
	Output: 1

**Example 2:**

	Input: nums = [2,2,2,0,1]
	Output: 0

**Constraints:**

-   `n == nums.length`
-   `1 <= n <= 5000`
-   `-5000 <= nums[i] <= 5000`
-   `nums`  is sorted and rotated between  `1`  and  `n`  times.

**Solution:**

```go
func findMin(nums []int) int {
    n := len(nums)
    lo,hi := 0,n-1
    
    if n == 1 { return nums[0] }
    if nums[lo] < nums[hi] { return nums[0] } // not reverted
    
    for lo < hi {
        mid := (lo+hi)/2
        if nums[mid] < nums[hi] {
            hi = mid // min on left side
        }else if nums[mid] > nums[hi] {
            lo = mid + 1 // min on right side
        }else {
            hi-- // duplicates
        }
    }
    
    return nums[hi]
}
```