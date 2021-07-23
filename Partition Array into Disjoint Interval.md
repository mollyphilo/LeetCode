# Partition Array into Disjoint Intervals

Given an array  `nums`, partition it into two (contiguous) subarrays `left` and  `right` so that:

-   Every element in  `left` is less than or equal to every element in  `right`.
-   `left`  and  `right`  are non-empty.
-   `left` has the smallest possible size.

Return the  **length**  of  `left`  after such a partitioning. It is guaranteed that such a partitioning exists.

**Example 1:**

	Input: nums = [5,0,3,8,6]
	Output: 3
	Explanation: left = [5,0,3], right = [8,6]

**Example 2:**

	Input: nums = [1,1,1,0,6,12]
	Output: 4
	Explanation: left = [1,1,1,0], right = [6,12]

**Note:**

1.  `2 <= nums.length <= 30000`
2.  `0 <= nums[i] <= 106`
3.  It is guaranteed there is at least one way to partition  `nums`  as described.

**Solution**

```go
func partitionDisjoint(nums []int) int {
    n := len(nums)
    localMax := make([]int,n)
    
    var max int = nums[0]
    for i := 0; i < n; i++ {
        if nums[i] > max { max = nums[i] }
        localMax[i] = max
    }
    var min int = nums[n-1]
    for i := n-1; i >= 0; i-- {
        if nums[i] < min { min = nums[i] }
        nums[i] = min
    }
    
    for i := 0; i < n-1; i++ {
        if localMax[i] <= nums[i+1] { return i+1 }
    }
    
    return -1
}
```