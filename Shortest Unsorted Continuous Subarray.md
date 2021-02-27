# Shortest Unsorted Continuous Subarray
Given an integer array  `nums`, you need to find one  **continuous subarray**  that if you only sort this subarray in ascending order, then the whole array will be sorted in ascending order.

Return  _the shortest such subarray and output its length_.

**Example 1:**

	Input: nums = [2,6,4,8,10,9,15]
	Output: 5
	Explanation: You need to sort [6, 4, 8, 10, 9] in ascending order to make the whole array sorted in ascending order.

**Example 2:**

	Input: nums = [1,2,3,4]
	Output: 0

**Example 3:**

	Input: nums = [1]
	Output: 0

**Constraints:**

-   `1 <= nums.length <= 104`
-   `-105  <= nums[i] <= 105`

**Follow up:** Can you solve it in `O(n)` time complexity?

**Solution:**

Time complexity: O(NlogN), Space complexity: O(N)

```go
import "sort"

func findUnsortedSubarray(nums []int) int {
    n := len(nums)
    sortedNums := make([]int,n)
    for i := 0; i < n; i++ {
        sortedNums[i] = nums[i]
    }
    sort.Ints(sortedNums)

    start,end := n,-1
    for i := 0; i < n; i++ {
        if sortedNums[i] != nums[i] {
            start = min(start,i)
            end = max(end,i)
        }
    }
    
    if v := end - start; v >= 0 {
        return v + 1
    }
    return 0
}

func min(a,b int) int {
    if a < b { return a }
    return b
}

func max(a,b int) int {
    if a > b { return a }
    return b
}

```